---
layout: post
title: "Guava TimeLimiter 코드 재구현"
date: 2023-09-20
tags: [재, Guava, 코드, 구현, TimeLimiter]
categories: blog
---

## 사용자 입력:
> int nTimeOutSec = 30;
TimeLimiter limiter = new SimpleTimeLimiter(Executors.newSingleThreadExecutor());
if ( (x = limiter.callWithTimeout(
    				new Callable<String>() {
		            	@Override
		            	public String call() throws Exception {
		            		return input.readLine();
		            	}
		            }
    				, nTimeOutSec, TimeUnit.SECONDS, true)) != null) {
					
					// something
					}
위와 같이 guava-20.0.jar을 사용하는 코드가 있는데, 취약점 보완때문에 상위라이브러리를 써야 해. 그런데, 상위라이브러리에는 SimpleTimeLimiter 클래스가 deprecated되어 없어. 그래서, 위 코드를 완벽하게 재현하면서 취약점이 없는 java코드로 만들어봐.

## ChatGPT 응답:
> `SimpleTimeLimiter`가 deprecated된 이후의 Guava 버전에서는 `TimeLimiter` 인터페이스만 남아 있으며, 이를 구현하기 위해서는 다른 방법을 사용해야 합니다.

다음은 Guava의 `TimeLimiter`를 사용하여 위의 코드를 재구현하는 방법입니다:

```java
import com.google.common.util.concurrent.TimeLimiter;
import com.google.common.util.concurrent.Uninterruptibles;
import com.google.common.util.concurrent.SimpleTimeLimiter;

import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.util.concurrent.Callable;
import java.util.concurrent.Executors;
import java.util.concurrent.TimeUnit;

public class TimeLimitExample {

    public static void main(String[] args) {
        int nTimeOutSec = 30;
        TimeLimiter timeLimiter = SimpleTimeLimiter.create(Executors.newSingleThreadExecutor());
        BufferedReader input = new BufferedReader(new InputStreamReader(System.in));

        try {
            String x = timeLimiter.callWithTimeout(new Callable<String>() {
                @Override
                public String call() throws Exception {
                    return input.readLine();
                }
            }, nTimeOutSec, TimeUnit.SECONDS);

            if (x != null) {
                // something
                System.out.println("Input received: " + x);
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

주의:
1. `SimpleTimeLimiter.create` 메서드를 사용하여 `TimeLimiter` 인스턴스를 생성합니다.
2. 위의 코드는 `BufferedReader`를 사용하여 표준 입력에서 데이터를 읽습니다.
3. 예외 처리를 적절하게 추가해야 할 수도 있습니다. 위의 예제에서는 간단하게 예외를 출력하도록 했습니다.

코드를 실행하면 사용자로부터 입력을 기다리며, 지정된 시간 동안 입력이 없으면 `TimeoutException`이 발생합니다.

## 사용자 입력:
> SimpleTimeLimtier클래스는 없어. 다시 만들어.

## ChatGPT 응답:
> 죄송합니다. 잘못된 정보를 제공했습니다.

`SimpleTimeLimiter`가 더 이상 사용되지 않으므로 직접 스레드를 사용하여 타임아웃을 처리해야 합니다. 다음은 `ExecutorService`와 `Future`를 사용하여 주어진 시간 내에 작업을 완료하려는 시도를 나타내는 방법입니다.

```java
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.util.concurrent.Callable;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;
import java.util.concurrent.Future;
import java.util.concurrent.TimeUnit;
import java.util.concurrent.TimeoutException;

public class TimeLimitExample {

    public static void main(String[] args) {
        int nTimeOutSec = 30;
        ExecutorService executor = Executors.newSingleThreadExecutor();
        BufferedReader input = new BufferedReader(new InputStreamReader(System.in));

        Future<String> future = executor.submit(new Callable<String>() {
            @Override
            public String call() throws Exception {
                return input.readLine();
            }
        });

        try {
            String x = future.get(nTimeOutSec, TimeUnit.SECONDS);

            if (x != null) {
                // something
                System.out.println("Input received: " + x);
            }
        } catch (TimeoutException e) {
            System.out.println("Timeout occurred!");
            future.cancel(true);
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            executor.shutdown();
        }
    }
}
```

이 코드는 `ExecutorService`를 사용하여 입력 작업을 제출하고 `future.get()`을 사용하여 결과를 기다립니다. 만약 결과가 제한 시간 내에 도달하지 않으면 `TimeoutException`이 발생하고 작업이 취소됩니다.

## 사용자 입력:
> 위 코드는 java8에서 잘 실행될 수 있나?

## ChatGPT 응답:
> 네, 제가 제공한 코드는 Java 8에서 실행될 수 있습니다. Java 8에서는 `ExecutorService`, `Future`, `Callable` 등의 클래스와 인터페이스가 이미 존재하므로 해당 코드를 Java 8 환경에서 실행하는 데 문제가 없어야 합니다.

## 사용자 입력:
> 그러면, 위와 같이 만들지 말고 Deprecated된 SimpleTimeLimiter의 기능 그대로, 클래스를 만들어줘.

## ChatGPT 응답:
> `SimpleTimeLimiter`의 핵심 기능을 따라 재현하는 클래스를 작성하겠습니다. 아래는 `SimpleTimeLimiter`의 핵심 기능을 따라한 클래스입니다:

```java
import java.util.concurrent.*;

public class CustomTimeLimiter {

    private final ExecutorService executor;

    public CustomTimeLimiter() {
        this.executor = Executors.newSingleThreadExecutor();
    }

    public <T> T callWithTimeout(Callable<T> callable, long timeoutDuration, TimeUnit timeoutUnit, boolean interruptIfRunning) throws Exception {
        Future<T> task = executor.submit(callable);

        try {
            return task.get(timeoutDuration, timeoutUnit);
        } catch (TimeoutException e) {
            if (interruptIfRunning) {
                task.cancel(true);
            }
            throw e;
        } catch (ExecutionException e) {
            // Unwrap the cause of the ExecutionException if possible
            Throwable cause = e.getCause();
            if (cause instanceof Exception) {
                throw (Exception) cause;
            } else {
                throw e;
            }
        }
    }

    public void shutdown() {
        executor.shutdown();
    }
}
```

이 클래스를 사용하는 방법은 다음과 같습니다:

```java
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.util.concurrent.Callable;
import java.util.concurrent.TimeUnit;

public class TimeLimitExample {

    public static void main(String[] args) {
        int nTimeOutSec = 30;
        CustomTimeLimiter timeLimiter = new CustomTimeLimiter();
        BufferedReader input = new BufferedReader(new InputStreamReader(System.in));

        try {
            String x = timeLimiter.callWithTimeout(new Callable<String>() {
                @Override
                public String call() throws Exception {
                    return input.readLine();
                }
            }, nTimeOutSec, TimeUnit.SECONDS, true);

            if (x != null) {
                // something
                System.out.println("Input received: " + x);
            }
        } catch (TimeoutException e) {
            System.out.println("Timeout occurred!");
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            timeLimiter.shutdown();
        }
    }
}
```

이 코드는 `CustomTimeLimiter` 클래스를 사용하여 입력을 제한된 시간 내에 읽습니다. 만약 입력이 제한된 시간 내에 도달하지 않으면 `TimeoutException`이 발생합니다.

## 사용자 입력:
> 생성자에 java.util.concurrent.Executors.newSingleThreadExecutor() 를 받아서 처리하는 식으로 만들어줘.

## ChatGPT 응답:
> 네, 생성자에 `ExecutorService`를 인자로 받아 처리하도록 변경하겠습니다.

```java
import java.util.concurrent.*;

public class CustomTimeLimiter {

    private final ExecutorService executor;

    public CustomTimeLimiter(ExecutorService executorService) {
        this.executor = executorService;
    }

    public <T> T callWithTimeout(Callable<T> callable, long timeoutDuration, TimeUnit timeoutUnit, boolean interruptIfRunning) throws Exception {
        Future<T> task = executor.submit(callable);

        try {
            return task.get(timeoutDuration, timeoutUnit);
        } catch (TimeoutException e) {
            if (interruptIfRunning) {
                task.cancel(true);
            }
            throw e;
        } catch (ExecutionException e) {
            // Unwrap the cause of the ExecutionException if possible
            Throwable cause = e.getCause();
            if (cause instanceof Exception) {
                throw (Exception) cause;
            } else {
                throw e;
            }
        }
    }

    public void shutdown() {
        executor.shutdown();
    }
}
```

사용 예:

```java
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.util.concurrent.Callable;
import java.util.concurrent.Executors;
import java.util.concurrent.TimeUnit;

public class TimeLimitExample {

    public static void main(String[] args) {
        int nTimeOutSec = 30;
        CustomTimeLimiter timeLimiter = new CustomTimeLimiter(Executors.newSingleThreadExecutor());
        BufferedReader input = new BufferedReader(new InputStreamReader(System.in));

        try {
            String x = timeLimiter.callWithTimeout(new Callable<String>() {
                @Override
                public String call() throws Exception {
                    return input.readLine();
                }
            }, nTimeOutSec, TimeUnit.SECONDS, true);

            if (x != null) {
                // something
                System.out.println("Input received: " + x);
            }
        } catch (TimeoutException e) {
            System.out.println("Timeout occurred!");
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            timeLimiter.shutdown();
        }
    }
}
```

이렇게 `CustomTimeLimiter`의 생성자에 `ExecutorService`를 전달하여 사용할 수 있습니다.

## 사용자 입력:
> 위에 코드에서  TimeLimiter 인터페이스는 적용해서 만들어줄래?

## ChatGPT 응답:
> 물론입니다. 먼저 `TimeLimiter` 인터페이스를 정의하겠습니다. 그 후에 `CustomTimeLimiter` 클래스에서 이 인터페이스를 구현하도록 하겠습니다.

```java
import java.util.concurrent.*;

public interface TimeLimiter {
    <T> T callWithTimeout(Callable<T> callable, long timeoutDuration, TimeUnit timeoutUnit, boolean interruptIfRunning) throws Exception;
    void shutdown();
}

public class CustomTimeLimiter implements TimeLimiter {

    private final ExecutorService executor;

    public CustomTimeLimiter(ExecutorService executorService) {
        this.executor = executorService;
    }

    @Override
    public <T> T callWithTimeout(Callable<T> callable, long timeoutDuration, TimeUnit timeoutUnit, boolean interruptIfRunning) throws Exception {
        Future<T> task = executor.submit(callable);

        try {
            return task.get(timeoutDuration, timeoutUnit);
        } catch (TimeoutException e) {
            if (interruptIfRunning) {
                task.cancel(true);
            }
            throw e;
        } catch (ExecutionException e) {
            // Unwrap the cause of the ExecutionException if possible
            Throwable cause = e.getCause();
            if (cause instanceof Exception) {
                throw (Exception) cause;
            } else {
                throw e;
            }
        }
    }

    @Override
    public void shutdown() {
        executor.shutdown();
    }
}
```

사용 예제:

```java
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.util.concurrent.Callable;
import java.util.concurrent.Executors;
import java.util.concurrent.TimeUnit;

public class TimeLimitExample {

    public static void main(String[] args) {
        int nTimeOutSec = 30;
        TimeLimiter timeLimiter = new CustomTimeLimiter(Executors.newSingleThreadExecutor());
        BufferedReader input = new BufferedReader(new InputStreamReader(System.in));

        try {
            String x = timeLimiter.callWithTimeout(new Callable<String>() {
                @Override
                public String call() throws Exception {
                    return input.readLine();
                }
            }, nTimeOutSec, TimeUnit.SECONDS, true);

            if (x != null) {
                // something
                System.out.println("Input received: " + x);
            }
        } catch (TimeoutException e) {
            System.out.println("Timeout occurred!");
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            timeLimiter.shutdown();
        }
    }
}
```

이렇게 `CustomTimeLimiter`는 `TimeLimiter` 인터페이스를 구현하여 해당 기능을 제공합니다.

## 사용자 입력:
> com.google.common.util.concurrent.TimeLimiter를 적용해줘.

## ChatGPT 응답:
> 알겠습니다. `com.google.common.util.concurrent.TimeLimiter`를 사용하도록 코드를 수정하겠습니다. 

`TimeLimiter` 인터페이스에는 `callWithTimeout`와 `newProxy` 두 개의 메소드가 정의되어 있습니다. 여기서는 `callWithTimeout` 메소드만 구현하도록 하겠습니다.

```java
import com.google.common.util.concurrent.TimeLimiter;

import java.util.concurrent.*;

public class CustomTimeLimiter implements TimeLimiter {

    private final ExecutorService executor;

    public CustomTimeLimiter(ExecutorService executorService) {
        this.executor = executorService;
    }

    @Override
    public <T> T callWithTimeout(Callable<T> callable, long timeoutDuration, TimeUnit timeoutUnit) throws TimeoutException, InterruptedException, ExecutionException {
        Future<T> task = executor.submit(callable);

        try {
            return task.get(timeoutDuration, timeoutUnit);
        } catch (TimeoutException e) {
            task.cancel(true);
            throw e;
        }
    }

    @Override
    public <T> T newProxy(T targetProxy, Class<T> interfaceType, long timeoutDuration, TimeUnit timeoutUnit) {
        throw new UnsupportedOperationException("newProxy method is not supported.");
    }

    public void shutdown() {
        executor.shutdown();
    }
}
```

사용 예제:

```java
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.util.concurrent.Callable;
import java.util.concurrent.Executors;
import java.util.concurrent.TimeUnit;
import java.util.concurrent.TimeoutException;

public class TimeLimitExample {

    public static void main(String[] args) {
        int nTimeOutSec = 30;
        TimeLimiter timeLimiter = new CustomTimeLimiter(Executors.newSingleThreadExecutor());
        BufferedReader input = new BufferedReader(new InputStreamReader(System.in));

        try {
            String x = timeLimiter.callWithTimeout(new Callable<String>() {
                @Override
                public String call() throws Exception {
                    return input.readLine();
                }
            }, nTimeOutSec, TimeUnit.SECONDS);

            if (x != null) {
                // something
                System.out.println("Input received: " + x);
            }
        } catch (TimeoutException e) {
            System.out.println("Timeout occurred!");
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            ((CustomTimeLimiter) timeLimiter).shutdown();
        }
    }
}
```

이제 `CustomTimeLimiter`는 Guava의 `TimeLimiter` 인터페이스를 구현하도록 수정되었습니다.

