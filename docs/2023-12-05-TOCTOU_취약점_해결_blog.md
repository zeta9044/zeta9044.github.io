---
layout: post
title: "TOCTOU 취약점 해결"
date: 2023-12-05
tags: [해결, 취약점, TOCTOU]
categories: blog
---

## 사용자 입력:
> class InvokeProcess extends Thread {
		private String prjId = "";
		private String fileName = "";
		private String scheduleId = "";
		private Process process = null;

		public InvokeProcess(String startDateTime, String prjId, String jobId, String scheduleId){
			this.prjId = prjId;

			// 2009.11.06. LeeYH. Add... -- BEGIN
			// OS 계열에 따라 실행 확장자를 다르게 처리한다.
			if ( isWindow ){
				this.fileName = systemHome+"/bin/" + prjId +"_run.bat";
			}else {
				this.fileName = systemHome+"/bin/" + prjId +"_run.sh";
			}
//			this.fileName = systemHome+"/bin/" + prjId +"_run.sh";
			// 2009.11.06. LeeYH. Add... -- END

			this.scheduleId    = scheduleId;
		}

		public String getFileName(){
			return this.fileName;
		}

		public void run(){
			String startDateTime = "";				
			try {
				Cronlog.info( "[ProjectID=" + prjId + "] Process Invoke Success...");

				File f = new File(this.fileName);
				// 분석시 Paser 종류가 변경이 가능하므로 새롭게 실행될때마다  shell를 다시 만들어야 확인할 수 있어서 존재체크를 삭제함.
				if ( f.exists() )    f.delete();		// 2019.10.19. LeeYH. Modify... -- 존재하면 기존것을 지우고 새롭게 만든다.
				MakeShellFile makeFile = new MakeShellFile();
				String fname = makeFile.make(this.prjId,this.scheduleId);

				if ( !fname.equals("") )	// 2015.11.11. LeeYH. Add... -- 실행 shell을 만든 경우에만 처리하도록 변경.
				{
					Cronlog.info( "[ProjectID=" + prjId + "] [Run File Name:"+fileName+"] ");

					startDateTime = ProcessLogger.getStartDateTime();

					// OS 계열에 따라 실행 확장자를 다르게 처리한다.
					if ( isWindow ){
						process = new ProcessBuilder( "batch", this.fileName ).start();
					}else {
						process = new ProcessBuilder( "sh", this.fileName ).start();
					}
					// 2023.06.05. LeeYH. Add...
					// (중요) pid = -1 이 발생하여 로그찍는곳을 옮기낟.
					if ( process != null && !"".equals(startDateTime) )
					{
				        // JDK 1.8에서는 Process 클래스에 'pid' 필드가 없으므로 리플렉션을 사용하여 접근합니다.
				        Field pidField = process.getClass().getDeclaredField("pid");
				        pidField.setAccessible(true);
				        int pid = (int) pidField.get(process);
				        
						Cronlog.info( "[ProjectID=" + prjId + "] Invoke Process ID : [" + pid +"]" );
						ProcessLogger.startResult( startDateTime, prjId, scheduleId, pid );
					}
				}
				else
				{
					Cronlog.info( "[ProjectID=" + prjId + "] The run shell failed to create file...");
				}

			}catch (Throwable t){
				Cronlog.error( "[ProjectID=" + prjId + "] [Exception]:", t);
			}
		}
	}  이 자바클래스의 run함수에서 Time-Of-Check to Time-Of-Use (TOCTOU) 레이스 컨디션 취약점을 해결해줘.

## ChatGPT 응답:
> 이 Java 클래스의 `run` 함수에서 Time-Of-Check to Time-Of-Use (TOCTOU) 레이스 컨디션 취약점을 해결하기 위해 몇 가지 수정이 필요합니다. TOCTOU 레이스 컨디션은 파일이나 리소스가 체크된 시점과 사용되는 시점 사이에 변경될 수 있는 환경에서 발생합니다. 여기서 문제는 파일의 존재 여부를 확인하고 나중에 해당 파일을 사용하는 부분에 있습니다.

### 원본 코드의 문제점

1. `f.exists()`를 통해 파일의 존재 여부를 확인하고, 존재한다면 `f.delete()`를 호출하는 부분이 있습니다. 이 사이의 시간 간격 동안 다른 프로세스나 스레드가 해당 파일을 수정할 수 있으며, 이로 인해 취약점이 발생합니다.
2. 파일을 삭제한 후 새로운 파일을 생성하는 과정도 동일한 TOCTOU 취약점을 가질 수 있습니다.

### 해결 방안

1. **원자적 작업(Atomic Operation)**: 파일의 확인과 삭제를 원자적으로 수행해야 합니다. Java의 `File` 클래스는 이러한 원자적 작업을 직접 지원하지 않으므로, 대안적인 방법을 모색해야 합니다. 예를 들어, 파일 시스템의 잠금 기능을 활용하거나, 파일을 다룰 때 동시성을 관리할 수 있는 다른 Java 라이브러리를 사용할 수 있습니다.

2. **경쟁 조건 회피**: 파일을 처리하기 전에 잠금을 설정하고 작업이 완료된 후에 잠금을 해제하는 방식으로 경쟁 조건을 회피할 수 있습니다. 이 방법은 다른 프로세스가 동시에 같은 파일에 접근하는 것을 방지합니다.

3. **트랜잭션 파일 시스템 사용**: 파일 작업을 트랜잭션으로 처리하여, 작업 중간에 발생할 수 있는 예기치 못한 변경을 방지할 수 있습니다. 이는 보다 복잡한 구현을 필요로 하지만, 더 강력한 안정성을 제공합니다.

4. **로깅 및 예외 처리 강화**: 예외 발생 시 적절한 처리와 로깅을 통해 문제가 발생했을 때 빠르게 대응할 수 있도록 합니다. 이는 직접적인 해결책은 아니지만, 문제 해결에 도움이 될 수 있습니다.

### 수정된 코드 예시

아래는 수정된 코드의 예시입니다. 이 예시에서는 파일의 존재 여부를 확인하고 삭제하는 부분에 대한 안전한 처리가 필요합니다. 실제 구현에서는 위의 해결 방안을 고려하여 추가적인 조치가 필요할 수 있습니다.

```java
public void run(){
    // ... 기존 코드 ...
    try {
        File f = new File(this.fileName);
        synchronized(this) {  // 동기화 블록 시작
            if (f.exists()) { 
                // 파일 삭제 전에 다른 작업이 개입하지 못하도록 잠금 설정
                f.delete();
            }
            // 파일 작업 수행
            // ... 파일 작업 코드 ...
        }  // 동기화 블록 종료
        // ... 기존 코드 ...
    } catch (Throwable t) {
        // ... 예외 처리 ...
    }
}
```

이 코드는 단순한 예시로, 실제 적용시에는 프로젝트의 전체적인 구조와 요구 사항에 맞게 조정해야 합니다.

## 사용자 입력:
> 동기화 블록을 어디까지 하는지 자세히.

## ChatGPT 응답:
> 동기화 블록을 사용할 때, 그 범위를 어떻게 설정하는지는 중요한 부분입니다. 동기화 블록의 목적은 공유 자원에 대한 동시 접근을 제어하여 경쟁 조건이나 레이스 컨디션을 방지하는 것입니다. 이 경우에는 파일 시스템에 대한 접근을 안전하게 관리해야 합니다.

### 동기화 블록 설정 방법

1. **최소 필요 범위**: 동기화는 필요한 최소한의 범위에만 적용되어야 합니다. 너무 넓은 범위에 적용하면 성능 저하의 원인이 될 수 있습니다. 반면, 너무 좁게 설정하면 취약점을 제대로 해결하지 못할 수 있습니다.

2. **공유 자원에 대한 접근 제어**: 이 예에서 공유 자원은 파일 시스템입니다. 파일의 존재 여부를 확인하고, 필요한 경우 삭제하고, 새로운 파일을 생성하는 과정 전체가 동기화되어야 합니다.

3. **동기화 블록의 범위**: 다음과 같은 순서로 동기화 블록을 설정합니다.
   - 파일의 존재 여부를 확인합니다.
   - 파일이 존재한다면 삭제합니다.
   - 새로운 파일을 생성합니다.

### 예시

```java
public void run(){
    try {
        synchronized(this) {  // 동기화 블록 시작
            File f = new File(this.fileName);
            if (f.exists()) { 
                f.delete(); // 파일 삭제
            }
            MakeShellFile makeFile = new MakeShellFile();
            String fname = makeFile.make(this.prjId, this.scheduleId); // 새 파일 생성

            if (!fname.equals("")) {
                // 파일 생성 성공시 추가 작업 수행
                // ... 추가 작업 코드 ...
            }
        }  // 동기화 블록 종료
    } catch (Throwable t) {
        // 예외 처리
        // ... 예외 처리 코드 ...
    }
}
```

### 주의 사항

- 동기화 블록 안에서 I/O 작업을 수행할 때는 성능 저하를 고려해야 합니다. 가능하다면 I/O 작업을 최소화하고, 필요한 경우 다른 최적화 방법을 고려해야 합니다.
- 동기화 블록이 너무 길어지면 다른 스레드의 대기 시간이 길어져 시스템 전체의 성능에 영향을 줄 수 있습니다. 따라서 동기화는 신중하게 적용해야 합니다.
- 여러 스레드가 동시에 파일에 접근하는 환경에서는 동기화 외에도 파일 잠금 같은 추가적인 방법을 고려해야 할 수도 있습니다.

