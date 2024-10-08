---
title: Spring Batch 적용기
author: park
date: 2023-10-11 15:25:58 +0800
categories: [Java, Programming]
tags: [typography]
math: true
mermaid: true
published: true # 포스팅 개시할 때, 바로 반영되는 옵션
# image: 

---

<br>

## Spring Batch 적용기

<br>
● 목표와 기술 선택<br>
● 구현, 문제 발생<br>
● 소감<br>
<br>

---

<br>

## ● 목적과 기술 선택

<br>
우선 Spring Batch 를 적용하는 목표가 중요했다.<br>
토이 프로젝트인 Snack 을 진행하던 도중, 구독 서비스를 적용하고 싶었다.<br>
주문 시, 구독을 요청하면 신청한 패턴의 날짜마다 배달을 하는 서비스였다.<br>
그렇게 성립된 배치를 구현하는 목표.<br>
<br>
<b>목표: 매일 자정, 구독을 요청한 사용자 중, 신청한 특정 패턴의 날짜마다 간식을 배달해주는 서비스 구현</b><br>
<br>
목적은 확실하게 정해졌다.<br>
물론 구현의 목표만 봐도 엔티티 한, 두 개로 해결될 기능은 아닐 것으로 보였다.<br>
또한 구독을 요청하는 일반적인 서비스 로직과 배치에서 동작할 서비스 로직은 전혀 다른 관심사를 갖지만 동일하거나 파생된 엔티티의 관리를 해야할 것이다.<br>
<br>
그렇다면 사용될 기술은 어떻게 될것인가?<br>
우선 스케쥴링을 관리하는 기술을 스프링 스케줄러를 이용할 것이다.<br>
동작하는 스케줄링은 Cron 표현식을 통해 등록하려 한다.<br>
<br>

Cron 표현식 Generator: [Cron생성기](http://www.cronmaker.com/?1)

<br>
일전에 현업에서 쿼츠를 이용한 적이 있다.<br>
하지만 신입과 다름이 없을 때 였고, 공공 API 와 호출로 연계가 필요했던 작업이라 정말 어려웠다.<br>
그래서 악몽을 떨쳐내고자 스프링 스케줄러를 선택했다.<br>
물론 당시 작업이 절대 쉽지 않은 것은 맞았지만, 지금봐도 스케줄러보다 어려웠던 기억이 있다.<br>
<br>
즉, 스프링 배치, 스프링 스케줄러를 이용할 것이며 Reader, Processor, Writer 를 구현한 Step 을 통해 Job 을 등록할 것이다.<br>
또한 Data Access Layer 와 소통하는 것은 JPA 가 될것이다.<br>
Reader 에서는 JPQL 을 이용하려 한다.<br>
<br>

---

<br>

## ● 구현, 문제 발생

<br>
우선 Reader 에서 JPQL 을 이용하려 했던 점은 조금 고생이었다.<br>
native 쿼리도 아닌 JPQL 이라 쿼리의 어느 부분에서 에러가 났는지 확실하게 파악이 힘들었다.<br>
그리고 조건이 몇 가지 필요했었다.<br>
목적에서 밝힌 <b>'사용자가 신청한 특정 패턴의 날짜'</b> 라는 것이 중요 조건이었다.<br>
패턴의 예시로 들면 이러했다.<br>
<br>
매일<br>
매주 월요일<br>
매달 1일<br>
<br>
등등<br>
이러한 패턴으로 구독 서비스를 제공했다.<br>
<br>

### ○ 첫 번째 문제. Reader

처음 겪은 문제는 Reader 의 JPQL 조건절에서 발생했다.<br>
쿼리의 논리 조건자인 AND 와 OR 조건을 기호를 이용해서 표현했었다.<br>
마치 MyBatis 의 그것처럼.<br>
<br>
그리고 &#124;&#124; 연산자가 제대로 동작하지 않는 것을 확인했다.<br>
Hibernate 는 &#124;&#124; 연산자를 제대로 읽지 못 했다.<br>
<br>

```sql

select s
from Subscribe s
where s.endYn = 'N'                                     -- 종료 여부
    and (                                               -- s.cycle: 구독과 관계를 갖는 패턴 엔티티 
        (s.cycle.id = 1)                                -- (1: 매일, 2: 매주 월요일, 3: 매월 1일 등)
        || (s.cycle.id = 2 and (weekday(now()) = 0))    -- weekday(): 요일의 index 를 알려주는 함수
        || (s.cycle.id = 3 and (date_format(now(), '%d') = '01'))            -- 매월 1일 인지 확인
        -- 이하 생략
    )

```

<br>
위처럼 패턴을 따로 관리하여 조건에 맞춰 구독을 조회했다.<br>
하지만,<br>
<br>

```sql

-- 조건절 제외 생략

where
    subscribe0_.end_yn=? 
    and subscribe0_2_.cycle_id=concat(1)

```

<br>
이렇게 &#124;&#124; 연산자를 통한 or 조건의 쿼리를 제대로 생성하지 못 했다.<br>
그 이후 <b>or 와 &#124;&#124; 연산자를 혼용하여 JPQL 쿼리를 작성</b>해봤다.<br>
그리고 예외가 발생했으며 뱉어낸 예외 는<br>
<br>

```bash

unexpected AST node: ||

```

<br>
즉, **&#124;&#124; 기호를 이용하지 못 한다**는 소리였다.<br>
이를 통해 &#124;&#124; 를 명시적으로 **OR 로 바꿔주니 정상 동작** 하였다.<br>
<br>

---

### ○ 두 번째 문제. Writer

우선 내가 원하는 상태 변화의 조건은 이랬다.<br>
<br>
Subscribe, SubscribeDetail 은 상태가 변화될 수 있는 엔티티,<br>
Cycle 은 조건 중 하나로 동작하는 불변 엔티티이다.<br>
편의상<br>
<br>
<b>
Subscribe -> 구독<br>
SubscribeDetail -> 구독 상세<br>
Cycle -> 패턴<br>
</b>
<br>
으로 표현하겠다.<br>
<b>구독과 구독 상세는 1 대 1</b> 관계를 가지며, <b>구독과 패턴도 1 대 1</b> 관계를 가진다.<br>
그리고 <b>모두 구독이 주인</b>이 되는 관계이다.<br>
<br>
프로세스는 이러하다.<br>
<br>

1. 구독의 종료 여부와 패턴의 id 을 기준으로 배달할 데이터를 조회한다.<br>
2. 구독 상세의 남은 횟수를 확인하고 감소한다.<br>
3. 남은 횟수가 0 에 달하면 구독의 종료 여부를 종료 값으로 수정한다.<br>
4. persist, fetch<br>

<br>
우선 배달은 아직 구현이 덜 된 부분이며, 프로세스는 Soft Delete 의 형태이다.<br>
조건에 맞지 않는 데이터는 더 조회되지 않는다.<br>
그리고 조건에 맞춰 모든 동작을 구현했다.<br>
<br>

```java

/**
 * @see 스케줄러 클래스
 */
@EnableScheduling
@Configuration
public class BatchSchedule {
    
    private final Logger LOGGER = LoggerFactory.getLogger(BatchSchedule.class);
    
    private final ApplicationContext context;
    
    private final JobLauncher jobLauncher;
    
    int index = 1;
    
    public BatchSchedule(ApplicationContext context, JobLauncher jobLauncher) {
        this.context = context;
        this.jobLauncher = jobLauncher;
    }
    
    @Scheduled(cron = "0 0 0 1/1 * ? *")
    public void runSubscribeDeliveryBatch() throws JobInstanceAlreadyCompleteException, JobExecutionAlreadyRunningException, JobParametersInvalidException, JobRestartException {
        LOGGER.info(">>>>>>>>>>>>>>>>>>>>>>> START Scheduled Subscribe Job <<<<<<<<<<<<<<<<<<<<<<<");

        JobParameters jobParameters = new JobParametersBuilder()
                .addLong("time", System.currentTimeMillis())
                .toJobParameters();

        Job subscribeDelivery = (Job) context.getBean("updateSubscribe");
        JobExecution execution = jobLauncher.run(subscribeDelivery, jobParameters);
        BatchStatus status = execution.getStatus();
        
        LOGGER.info(status.toString());
        LOGGER.info(">>>>>>>>>>>>>>>>>>>>>>> FINISHED Scheduled Subscribe Job <<<<<<<<<<<<<<<<<<<<<<<");
    }
}

...

/**
 * @see 구독 배치 클래스
 */
@EnableBatchProcessing
@Configuration
public class SubscribeDeliveryBatch {
    
    private final JobBuilderFactory jobBuilderFactory;
    
    private final StepBuilderFactory stepBuilderFactory;
    
    private final EntityManagerFactory entityManagerFactory;
    
    private final SubscribeRepository subscribeRepository;      // 구독을 관리하는 JpaRepository
        
    public SubscribeDeliveryBatch(JobBuilderFactory jobBuilderFactory
            , StepBuilderFactory stepBuilderFactory
            , EntityManagerFactory entityManagerFactory
            , SubscribeRepository subscribeRepository) {
        this.jobBuilderFactory = jobBuilderFactory;
        this.stepBuilderFactory = stepBuilderFactory;
        this.entityManagerFactory = entityManagerFactory;
        this.subscribeRepository = subscribeRepository;
    }
    
    @Bean
    public Job updateSubscribe(@Qualifier("subscribeStep1") Step step) {
        return jobBuilderFactory.get("updateSubscribe")
                .start(step)
                .build();
    }

    @Bean
    public Step subscribeStep1(ItemReader<Subscribe> reader
            , ItemProcessor<Subscribe, Subscribe> processor
            , ItemWriter<Subscribe> writer) {
        return stepBuilderFactory.get("subscribeStep1")
                .<Subscribe, Subscribe>chunk(10)
                .reader(reader)
                .processor(processor)
                .writer(writer)
                .build();
    }

    // Reader
    @Bean
    public JpaCursorItemReader<Subscribe> jpaCursorItemReader() {
        return new JpaCursorItemReaderBuilder<Subscribe>()
                .name("jpaCursorItemReader")
                .entityManagerFactory(entityManagerFactory)
                .queryString("SELECT s \n" +
                        "FROM Subscribe s \n" +
                        "WHERE s.endYn = :endYn \n" +
                        "    and (s.cycle.id = 1 \n" +
                        "       or (s.cycle.id = 2 and (weekday(now()) = 0)) \n" +
                        "       or (s.cycle.id = 3 and (date_format(now(), '%d') = '01')) \n" +
                        "   )")
                .parameterValues(Map.of("endYn", YN.N.getCode()))
                .build();
    }
    
    // Processor
    @Bean
    public ItemProcessor<Subscribe, Subscribe> decreaseSubscribe() {
        return subscribe -> {
            if (!subscribe.isEnd()) {               // 종료 여부 확인
                int left = subscribe.decrease();    // 구독 - 구독 상세의 남은 횟수를 감소 후, 남은 횟수 반환
                if (left == 0) {
                    subscribe.end();                // 남은 횟수가 0 일 경우, 서비스 종료
                }
            }
            return subscribe;
        };
    }
    
    // Writer
    @Bean
    public ItemWriter<Subscribe> jpaCursorItemWriter() {
        return subscribeRepository::saveAll;        // 상태가 변화된 엔티티의 영속성 저장
    }
}

```

코드는 나름 만족 했다.<br>
JPQL 이 생각보다 지저분해져서 마음에 안 드는 점과<br>
구독 상세의 관리를 구독이 직접하는 것을 제외하면 괜찮았다.<br>
그것도 구독 상세의 관리를 주인에게 맡긴다고 생각하여 나쁘지 않은 접근이었다고 생각은 한다.<br>
하지만 문제가 발생했다.<br>
<br>
<b>하위 엔티티인 구독 상세의 상태가 저장되지 않는다.</b><br>
<br>
Insert 작업도 아니고 update 이다.<br>
혼자서 삽질을 여럿 해보았다.<br>
<br>
같은 영속성 컨텍스트 안에 있는지 확인하려 노력하고,<br>
JPQL 에 따로 표기하여 동일 트랜젝션에 적용 시키려 노력하고,<br>
혹시라도 1 대 1 관계의 주종을 잘못 설정했나 싶어 다시 잡고,<br>
심지어는 테이블의 구조도 '구독 - 구독 상세 브릿지 - 구독 상세' 에서 '구독 - 구독 상세' 로 상세의 외래키를 직접 관리하게 변경도 해보았다.<br>
그렇게 삽질을 이어가며 좋아하는 상사와 고민하던 중, 상사가 번뜩이며 말했다.<br>
<br>
<b style="color: blue;">"cascade 걸으셨어요?"</b><br>
<b>아</b>, 머리가 띵했다.<br>
기능 구현에 급급하여 엔티티 수정 정책을 제대로 설정하지 못 했다.<br>
<br>
JPA 를 알고 있는 지식을 동원해서 몇 시간의 노력을 쏟았는데<br>
<br>

```java

@OneToOne(cascade = CascadeType.ALL)

```

실험용으로 설정해본 위의 cascade 정책 코드 단 한 줄로 해결됐다.<br>
물론 기존 서비스 로직에서 구독 상세 따로 구독 따로 저장하던 로직에 수정이 조금 필요했지만,<br>
Batch 의 프로세스 코드가 복잡해지는 것보다 훨씬 나았다.<br>
같은 트랜잭션, 같은 영속성 컨텍스트 내에서 관리되니 코드의 리팩터링도 쉬워졌다.<br>
확실히 JPA 는 아는 만큼 유용하고 편리해진다.<br>
<br>
야생형 개발자인 내 입장에서 학자형의 장점을 추가하고 싶은 순간이었다.<br>
<br>

---

<br>

## ● 소감

<br>
구현을 마치고 이해를 하고 나니 어려운 점은 없었다.<br>
다만 이제야 한 Step 을 정리한 수준이니, 다음 스텝을 구현해야 할것이다.<br>
다음 스텝의 목표는 이러하다.<br>
<br>
<b>목표: 구독 데이터로 배달 데이터 생성</b><br>
<br>
물론 같은 영속성 컨텍스트 안에서 관리하면 더 쉬워질 것이라는 것을 안다.<br>
하지만 왠지 어렵게 구현해보고 싶다는, 그런 초보 개발자의 욕심이다.<br>
<br>
또한 조언을 해준 상사의 말에 빌리면, chunk 크기와 paging 의 크기 차이로 인해 문제가 발생하거나 dirty read 가 발생할 수 있다고 한다.<br>
이는 JPA 와 Batch 를 꾸준히 공부하다가 보면 알 수 있을 것이라고 생각된다.<br>