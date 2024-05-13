---
layout: post
title: "Quartz 작업 의존성 설정"
date: 2023-03-14
tags: [Quartz, 작업, 설정, 의존]
categories: blog
---

## 사용자 입력:
> quartz에 작업 의존성을 걸 수 있나?

## ChatGPT 응답:
> 네, Quartz에서는 작업 간의 의존성을 설정할 수 있습니다. 이를 통해 실행해야 할 작업의 순서와 타이밍을 지정할 수 있습니다.

Quartz에서는 작업간의 의존성을 "트리거"를 사용하여 설정합니다. 트리거는 작업이 실행되는 시기를 제어하는데 사용됩니다. 한 작업이 다른 작업의 실행을 시작하려면, 다른 작업의 트리거를 현재 작업의 트리거와 연결하여 의존성을 설정하면 됩니다.

의존성을 설정하면 Quartz는 다른 작업이 완료되기를 기다리고 해당 작업이 실행될 수 있는지 확인합니다. 만약 다른 작업이 아직 실행 중이거나 완료되지 않았다면, 해당 작업은 실행되지 않습니다. 이를 통해 작업 간의 순서와 타이밍을 조정할 수 있습니다.

예를 들어, A 작업이 B 작업에 의존하도록 설정하려면, A 작업의 트리거를 B 작업의 트리거와 연결하면 됩니다. 이렇게 하면 B 작업이 완료되기 전에 A 작업이 실행되지 않습니다.

## 사용자 입력:
> 예제

## ChatGPT 응답:
> 다음은 Quartz에서 작업 간 의존성을 설정하는 예제입니다.

```java
// 스케줄러 생성
SchedulerFactory sf = new StdSchedulerFactory();
Scheduler scheduler = sf.getScheduler();

// 작업 A 생성
JobDetail jobA = JobBuilder.newJob(MyJob.class)
    .withIdentity("jobA", "group1")
    .build();

// 작업 B 생성
JobDetail jobB = JobBuilder.newJob(MyJob.class)
    .withIdentity("jobB", "group1")
    .build();

// 작업 A의 트리거 생성
Trigger triggerA = TriggerBuilder.newTrigger()
    .withIdentity("triggerA", "group1")
    .startNow()
    .build();

// 작업 B의 트리거 생성
Trigger triggerB = TriggerBuilder.newTrigger()
    .withIdentity("triggerB", "group1")
    .startAt(DateBuilder.futureDate(10, IntervalUnit.SECOND))
    .build();

// 작업 B가 작업 A에 의존하도록 설정
triggerB = triggerB.forJob(jobA.getKey());

// 작업 A와 B를 스케줄러에 등록
scheduler.scheduleJob(jobA, triggerA);
scheduler.scheduleJob(jobB, triggerB);

// 스케줄러 실행
scheduler.start();
```

위 예제에서는 먼저 두 개의 작업(JobDetail)과 트리거(Trigger)를 생성합니다. 작업 A와 작업 B는 동일한 작업 클래스(MyJob)를 참조하고 있습니다. 작업 A는 즉시 실행되도록 설정되어 있고, 작업 B는 10초 후에 실행되도록 설정되어 있습니다.

그리고 작업 B의 트리거를 작업 A의 키(JobKey)와 연결하여 작업 B가 작업 A에 의존하도록 설정하고, 스케줄러에 작업 A와 B를 등록합니다. 이렇게 설정하면 작업 B는 작업 A가 완료될 때까지 실행되지 않습니다.

마지막으로, 스케줄러를 실행합니다. 이제 작업 A가 즉시 실행되고, 10초 후에 작업 B가 실행됩니다. 작업 B가 실행되기 전에 작업 A가 완료되지 않으면, 작업 B는 실행되지 않습니다.

## 사용자 입력:
> 위에 내용을 ui로 지정할 수 있도록 코딩해줘

## ChatGPT 응답:
> UI를 사용하여 Quartz에서 작업 간의 의존성을 설정하려면 일반적으로 작업 및 트리거를 생성하는 데 필요한 정보를 입력할 수 있는 입력 폼이 필요합니다. 또한, 작업 간의 의존성을 설정할 때는 두 개의 작업을 선택하고, 선택된 작업 간에 어떤 종류의 의존성을 설정할 것인지를 선택할 수 있는 UI 요소가 필요합니다.

다음은 간단한 예제 코드입니다. 이 예제 코드는 Quartz 작업을 UI에서 생성하고, 의존성을 설정할 수 있도록 해줍니다. 이 예제는 JavaFX UI 프레임워크를 사용하여 작성되었습니다.

```java
public class QuartzUI extends Application {

    private final List<JobDetail> jobs = new ArrayList<>();
    private final List<Trigger> triggers = new ArrayList<>();

    private JobDetail selectedJob;
    private ComboBox<JobDetail> jobComboBox;

    public static void main(String[] args) {
        launch(args);
    }

    @Override
    public void start(Stage primaryStage) throws Exception {
        // 스케줄러 생성
        SchedulerFactory sf = new StdSchedulerFactory();
        Scheduler scheduler = sf.getScheduler();

        // 작업 생성을 위한 UI 구성 요소 생성
        Label jobLabel = new Label("Job Name:");
        jobComboBox = new ComboBox<>();
        jobComboBox.getItems().addAll(jobs);
        jobComboBox.getSelectionModel().selectedItemProperty().addListener((observable, oldValue, newValue) -> {
            selectedJob = newValue;
        });
        Button addJobButton = new Button("Add Job");
        addJobButton.setOnAction(event -> {
            JobDetail job = JobBuilder.newJob(MyJob.class)
                    .withIdentity(UUID.randomUUID().toString(), "group1")
                    .build();
            jobs.add(job);
            jobComboBox.getItems().add(job);
            jobComboBox.getSelectionModel().select(job);
        });

        // 작업 간 의존성 설정을 위한 UI 구성 요소 생성
        Label dependsOnLabel = new Label("Depends On:");
        ComboBox<JobDetail> dependsOnComboBox = new ComboBox<>();
        dependsOnComboBox.getItems().addAll(jobs);
        ComboBox<String> dependencyComboBox = new ComboBox<>();
        dependencyComboBox.getItems().addAll("None", "Finish", "Success");
        Button setDependencyButton = new Button("Set Dependency");
        setDependencyButton.setOnAction(event -> {
            if (selectedJob != null) {
                JobDetail dependsOnJob = dependsOnComboBox.getSelectionModel().getSelectedItem();
                String dependencyType = dependencyComboBox.getSelectionModel().getSelectedItem();
                if ("Finish".equals(dependencyType)) {
                    triggers.add(TriggerBuilder.newTrigger()
                            .withIdentity(UUID.randomUUID().toString(), "group1")
                            .forJob(selectedJob)
                            .endAt(DateBuilder.futureDate(10, IntervalUnit.SECOND))
                            .build());
                } else if ("Success".equals(dependencyType)) {
                    triggers.add(TriggerBuilder.newTrigger()
                            .withIdentity(UUID.randomUUID().toString(), "group1")
                            .forJob(selectedJob)
                            .startAt(DateBuilder.futureDate(10, IntervalUnit.SECOND))
                            .build());
                }
                if (dependsOnJob != null) {
                    Trigger trigger = TriggerBuilder.newTrigger()
                            .withIdentity(UUID.randomUUID().toString(), "group1")
                            .forJob(selectedJob)
                            .endAt(DateBuilder

## 사용자 입력:
> 위에 UI 말고 웹UI로 해줘

## ChatGPT 응답:
> Quartz 작업을 웹 UI로 생성하고 작업 간의 의존성을 설정하려면, 일반적으로 작업 및 트리거를 생성하는 데 필요한 정보를 입력할 수 있는 웹 폼이 필요합니다. 또한, 작업 간의 의존성을 설정할 때는 두 개의 작업을 선택하고, 선택된 작업 간에 어떤 종류의 의존성을 설정할 것인지를 선택할 수 있는 웹 UI 요소가 필요합니다.

다음은 간단한 예제 코드입니다. 이 예제 코드는 Spring Boot와 Thymeleaf 템플릿 엔진을 사용하여 작성되었습니다.

```java
@Controller
public class JobController {

    @Autowired
    private Scheduler scheduler;

    @GetMapping("/jobs")
    public String showJobs(Model model) {
        List<JobDetail> jobs = scheduler.getJobKeys(GroupMatcher.anyGroup()).stream()
                .map(jobKey -> {
                    try {
                        return scheduler.getJobDetail(jobKey);
                    } catch (SchedulerException e) {
                        throw new RuntimeException(e);
                    }
                })
                .collect(Collectors.toList());
        model.addAttribute("jobs", jobs);
        return "jobs";
    }

    @PostMapping("/jobs")
    public String createJob(@RequestParam String jobName, @RequestParam String jobClass,
                            @RequestParam(required = false) String dependsOnJobName,
                            @RequestParam(required = false) String dependencyType) {
        try {
            JobDetail jobDetail = JobBuilder.newJob(Class.forName(jobClass))
                    .withIdentity(jobName, "group1")
                    .build();
            Trigger trigger = TriggerBuilder.newTrigger()
                    .withIdentity(jobName + "Trigger", "group1")
                    .startNow()
                    .build();
            scheduler.scheduleJob(jobDetail, trigger);
            if (dependsOnJobName != null && dependencyType != null) {
                JobKey dependsOnJobKey = new JobKey(dependsOnJobName, "group1");
                Trigger dependencyTrigger = null;
                switch (dependencyType) {
                    case "Finish":
                        dependencyTrigger = TriggerBuilder.newTrigger()
                                .withIdentity(jobName + "FinishDependencyTrigger", "group1")
                                .forJob(dependsOnJobKey)
                                .endAt(DateBuilder.futureDate(10, IntervalUnit.SECOND))
                                .build();
                        break;
                    case "Success":
                        dependencyTrigger = TriggerBuilder.newTrigger()
                                .withIdentity(jobName + "SuccessDependencyTrigger", "group1")
                                .forJob(dependsOnJobKey)
                                .startAt(DateBuilder.futureDate(10, IntervalUnit.SECOND))
                                .build();
                        break;
                }
                if (dependencyTrigger != null) {
                    scheduler.scheduleJob(dependencyTrigger);
                }
            }
            return "redirect:/jobs";
        } catch (ClassNotFoundException | SchedulerException e) {
            throw new RuntimeException(e);
        }
    }
}
```

위 코드에서 `/jobs` 경로는 작업 목록을 표시하는 페이지를 나타냅니다. `showJobs` 메소드는 스케줄러에서 작업 목록을 가져와 Thymeleaf 템플릿에 전달합니다. `createJob` 메소드는 웹 폼에서 입력한 작업 정보를 사용하여 작업과 트리거를 생성하고, 선택된 작

## 사용자 입력:
> 계속

## ChatGPT 응답:
> 업 간의 의존성을 설정합니다. 선택된 작업이 다른 작업에 의존하도록 설정되었으면, 선택된 작업과 의존 작업 간의 트리거를 생성하여 스케줄러에 추가합니다.

다음은 작업을 생성하는 Thymeleaf 템플릿 코드입니다.

```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Quartz Jobs</title>
</head>
<body>
    <h1>Quartz Jobs</h1>
    <table>
        <thead>
            <tr>
                <th>Name</th>
                <th>Class</th>
                <th>Depends On</th>
                <th>Dependency Type</th>
            </tr>
        </thead>
        <tbody>
            <tr>
                <form method="post" th:action="@{/jobs}">
                    <td><input type="text" name="jobName"></td>
                    <td><input type="text" name="jobClass"></td>
                    <td>
                        <select name="dependsOnJobName">
                            <option value="">None</option>
                            <option th:each="job : ${jobs}"
                                    th:value="${job.key.name}"
                                    th:text="${job.key.name}"></option>
                        </select>
                    </td>
                    <td>
                        <select name="dependencyType">
                            <option value="">None</option>
                            <option value="Finish">Finish</option>
                            <option value="Success">Success</option>
                        </select>
                    </td>
                    <td><button type="submit">Add Job</button></td>
                </form>
            </tr>
            <tr th:each="job : ${jobs}">
                <td th:text="${job.key.name}"></td>
                <td th:text="${job.jobClass.name}"></td>
                <td>
                    <select th:name="${job.key.name + '_dependsOnJobName'}">
                        <option value="">None</option>
                        <option th:each="depJob : ${jobs}"
                                th:value="${depJob.key.name}"
                                th:text="${depJob.key.name}"></option>
                    </select>
                </td>
                <td>
                    <select th:name="${job.key.name + '_dependencyType'}">
                        <option value="">None</option>
                        <option value="Finish">Finish</option>
                        <option value="Success">Success</option>
                    </select>
                </td>
            </tr>
        </tbody>
    </table>
</body>
</html>
```

위 코드에서는 Thymeleaf를 사용하여 작업 목록과 작업 추가 폼을 생성합니다. 작업 추가 폼에서는 작업 이름, 작업 클래스, 의존 작업 이름 및 의존성 유형을 입력할 수 있습니다. 작업 목록에서는 각 작업마다 의존 작업 이름과 의존성 유형을 선택할 수 있습니다.

위 코드를 실행하면 `/jobs` 경로로 접속하여 작업을 추가하고, 작업 간의 의존성을 설정할 수 있습니다. 작업이 추가되면 스케줄러에서 해당 작업

