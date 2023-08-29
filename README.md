# 1. Yammer 데이터 분석 프로젝트

  * yammer는 사내 SNS로 동료들과 소통할 수 있는 장소로 slack과 같은 서비스를 제공하고 있습니다.

  * yammer를 통해 동료들과 문서, 아이디어, 게시글 등을 작성하여 공유할 수 있는 공간입니다.

  * yammer가 제공하는 총 4개의 테이블을 활용하여 실제 한 기업의 데이터 분석가가 수행하는 단계인 **문제 정의**, **가설 정의**, **가설 검증**  
    절차를 진행 해 보는 프로젝트입니다.

# 2.1 User Engagement 감소

## 문제 정의

<image src = "https://github.com/wbin0718/google_analytics_dashboard/assets/104637982/60d77627-a9e7-4fa5-9f4c-117c39b407d7" width = 500>

* 먼저 첫 번째 주제인 WAU 감소한 가설을 설정하고 원인을 찾아보겠습니다.
* 위 그래프를 보면 7월 28일을 기점으로 WAU가 꾸준히 감소하고 있습니다.
* WAU가 감소하는 이유를 데이터를 통해 찾고자 합니다.

## 가설 설정

* 휴일이었나? -> 휴일이면 회사를 출근하지 않으니까 접속자 수가 적을 것입니다.
* 신규 가입자가 적어졌을까? -> 신규 사용자의 감소로 인해 WAU가 감소하는 것으로 보일 수도 있습니다.
* 특정 기능이 동작 오류가 발생했나? -> 특정 기능을 통해 사용자가 접속할 수 있는데, 이 기능이 제대로 동작되지 않아 사용자가 접속하지 못 한 것일 수도 있습니다.
* 로그 수집 도구 오류가 발생했나? -> 로그 수집 도구 문제로 인해 제대로 집계가 되지 못 한 것일 수도 있습니다.

날짜 확인 결과 휴일은 아니었으며, 사내 워크샵 등 개인 일정은 확인할 수 없어 휴일은 제외하고 진행하겠습니다.  
로그 수집 도구 오류 또한 제외하고 분석을 진행했습니다.

## 가설 검증

### 신규 사용자의 문제?

먼저 WAU가 감소한 이유를 신규 가입자의 감소로 부터 이어졌다는 것을 확인 해 보겠습니다.

<image src = "https://github.com/wbin0718/google_analytics_dashboard/assets/104637982/60d77627-a9e7-4fa5-9f4c-117c39b407d7" width = 500>

일을 기준으로 신규 가입자의 수를 나타낸 그래프입니다.  
그래프를 보면 정확히 신규 가입자가 감소했는지 판단하기는 어려운 것 같아 이전 주 대비 증가 및 감소 퍼센트를 확인 해 보겠습니다.

<image src = "https://github.com/wbin0718/yammer_data_analysis/assets/104637982/134d07ec-b527-47c6-a0b4-33ad5e13f77e" width = 500>

위 그래프를 보면 7월 28일 기준으로 신규 가입자 8월 4일 -19%가 감소를 했지만 8월 11일 주차부터는 다시 신규 가입자가 상승하고 있습니다.
따라서 신규 가입자가 어느 시점 감소는 있었지만 다시 증가하는 추세를 보이고 있어 꾸준히 감소하는 WAU의 원인은 아닌 것 같습니다.

### 기존 사용자의 문제?

신규 가입자가 감소하는 것이 아니라면 기존 사용하고 있던 사용자의 문제로 WAU가 감소하고 있는 것 같습니다.

신규 가입자의 문제가 아니므로 어떤 사용자들이 어려움을 겪는지 확인하기 위해 가입 주차로 코호트를 나누어 리텐션을 살펴보겠습니다.

<image src = "https://github.com/wbin0718/yammer_data_analysis/assets/104637982/bdf1699d-3451-47a1-8be1-21793af2fe72" width = 500>

가입 주차로 코호트를 나누고 9월 1일까지의 리텐션을 확인한 결과 가입한지 10주 이상된 사용자들의 리텐션 비율이 8월 4일부터 급격히 떨어지고 있습니다.

### 이메일의 문제?

* yammer는 가입한 고객들에게 일주일의 내용을 요약해서 이메일을 사용자에게 보내주고 사용자가 yammer를 접속하도록 유도하는 digest email 이벤트가 발생하고 있습니다.
* 신규 고객들은 다양한 마케팅 채널을 통해 접속하는 반면 기존 사용자는 마케팅 채널보다 이메일을 통해 접속하는 경우가 많다고 합니다.
* 그렇다면 장기 고객들이 주로 접속하는 이메일 채널에서 어떤 문제가 생긴 것은 아닐지 확인을 해 볼 필요가 있습니다.

<image src = "https://github.com/wbin0718/google_analytics_dashboard/assets/104637982/e2ec1f4a-bc6e-4b53-afe8-15ebb5fb351d" width = 500>

위 그래프를 보면 이메일과 관련된 이벤트가 얼만큼 발생했는 지를 보여주고 있습니다.
고객들이 email을 open한 이벤트는 감소하지 않고 꾸준히 증가하고 있습니다.
하지만 email을 클릭을 하고 이메일 안의 링크를 클릭하는 이벤트는 감소하고 있습니다.

즉, 고객들이 이메일을 오픈은 하지만 이메일 안의 링크를 통해 yammer로 접속은 하지 않고 있습니다.
조금 더 구체적으로 확인하기 위해 이메일을 오픈 클릭률과 이메일 내용 링크를 클릭한 클릭률을 살펴보겠습니다.

<image src = "https://github.com/wbin0718/google_analytics_dashboard/assets/104637982/e08400d3-f583-4ac4-9965-5629f2ab40d8" width = 500>

맨 아래 두 그래프를 보시면 이메일을 오픈 클릭률은 증가하다가 살짝 감소 후 일정한 수준을 유지하고 있는 반면,
이메일을 오픈하고 내용 링크를 클릭한 비율은 증가하다가 계속 감소하고 있는 것을 볼 수가 있습니다.

## 분석 결과

* 분석한 것을 토대로 WAU가 감소한 원인은 오래된 사용자들입니다.
* 오래된 사용자들은 주로 이메일을 통해 접속하게 됩니다. 이메일 오픈하는 클릭률은 상승하다가 일정 수준을 계속 유지하고 있지만, 링크를 누르고 yammer 랜딩 페이지로 접속하는 클릭률은 조금씩 감소하고 있습니다.
* 이 때, 사용자들이 이메일을 오픈은 잘 하지만 링크가 랜딩페이지로 연결하지 못하는 오류가 발생해서 WAU가 감소되는 현상이 일어나는 것 같습니다.

  -> 따라서 WAU가 감소하는 이유에 대한 해결방안을 제시하는 것이 아닌 데이터 분석가로써 어떤 문제가 발생하고 있고, 어떤 부분을 자세히 관심을 가져보면 좋을 것 같다는 의견을 제시할 수 있습니다.

# 2.2 검색 기능 분석

## 문제 정의

* yammer는 현재 서비스에 안주하지 않고, 더욱 개선된 서비스를 제공하려고 함.
* 프로덕트팀은 먼저 검색 기능을 더욱 발전시키고 싶지만, 엔지니어링팀은 검색 기능을 가장 먼저 개발할 충분한 가치가 있는 지 알기를 원함.
* 따라서 프로덕트 팀은 검색 기능이 가치가 있는지를 데이터를 통해 찾고, 어떤 부분을 개선하면 좋을지 찾고자 함.

## 가설 설정

* 사용자가 검색 기능을 통해 최종적으로 얻고자 하는 목표가 무엇인가? -> 검색 기능을 통해 원하는 결과를 빠르고 쉽게 찾기를 원함!

1. 사용자가 검색 기능을 사용을 하고 있는지 확인이 필요함!
2. 사용하고 있다면 얼마나 자주 하는 지 확인이 필요함!
3. 사용자들이 활용을 하고 있다면 어떻게 활용을 하고 있는지 확인이 필요함! -> 이 부분을 통해 현재 서비스의 문제점을 파악할 수 있음!

## 검색 기능의 사용

검색 기능과 관련된 이벤트가 얼마나 발생하는 지 확인할 필요가 있음!

먼저 관련된 이벤트가 무엇이 있고, 언제 이벤트 기록이 되는지 이해가 우선!

search_autocompletion: 사용자가 키워드를 검색하는 순간 자동 완성으로 아래 몇 가지 결과가 발생하고, 이때 그 결과 중 하나를 클릭해서 결과 페이지로 랜딩되었을 때 이벤트가 발생.  
search_run: 사용자가 키워드를 작성하고 '엔터'를 누르거나, 아래 있는 see all search results를 클릭하여 결과 페이지로 랜딩되었을 때 이벤트가 발생.  
search_click_x (x: 1~10): 검색 결과 페이지로 랜딩되었을 때 그 중 x 번째 게시글을 클릭해서 그 페이지로 랜딩되었을 때 이벤트가 발생.  

사용자들이 검색 기능을 얼마나 사용하는 지 확인!
<div>
 <image src = "https://github.com/wbin0718/google_analytics_dashboard/assets/104637982/4df34547-5ba9-4e94-9da1-edba52d72c64" width = 500>
 <image src = "https://github.com/wbin0718/google_analytics_dashboard/assets/104637982/dcb0fe4b-3dbd-45a0-a5fc-211280c0a63d" width = 500>
</div>

위 그래프를 보면 한번이라도 자동 완성 검색, 전체 검색 기능을 사용한 세션이 증가하고 있음!  
특히 전체 세션 중 25%가량이 자동 완성 검색 기능을 사용하고 있음! -> 회사의 기준이 있겠지만 yammer는 25%면 사용자들 필요성을 느끼고 있다고 판단!

## 얼마나 자주 사용하고 있는가?

위에서 25% 세션이 자동 완성 검색 기능을 사용하고 있다고 판단!    
그렇다면 한 세션 내 얼마나 자주 사용하고 있는지 확인해 볼 필요가 있음!  

<div>
<image src = "https://github.com/wbin0718/google_analytics_dashboard/assets/104637982/7f2fe889-ffb7-42e0-8636-7a2ceb6b7de6" width = 500>
<image src = "https://github.com/wbin0718/yammer_data_analysis/assets/104637982/0916646a-e5ca-4214-bf90-ee80e6abefa5" width = 500>
</div>

위 그래프를 보면 왼쪽 그래프를 보면 자동 완성 기능은 한 세션당 많아야 2번 정도 사용하는 것으로 보임!  
오른쪽 그래프를 보면 전체 검색은 한 세션당 5번까지 사용하는 것으로 보임!

위 결과가 뜻하는 것이 무엇일까? -> 자동 완성 기능을 통해서는 한 세션 내에서 원하는 결과를 바로 찾을 수 있었지만, 전체 검색을 통해서는 원하는 결과를 바로 찾지 못해 검색을 다시 해 보는 것을 추축할 수 있음!  

## 클릭을 얼마나 했는가?

위에서 전체 검색을 통해서는 원하는 결과를 바로 찾지 못해 자동 완성 기능보다 한 세션 내에서 더 많이 사용하는 것을 추측할 수 있었음!  

조금 더 깊게 분석하기 위해 검색 후 클릭을 얼마나 했는지 확인해볼 필요가 있음!

<div>
 <image src = "https://github.com/wbin0718/google_analytics_dashboard/assets/104637982/02d7172f-2beb-4a16-bec8-dd92be9d1dea" width = 500>
 <image src = "https://github.com/wbin0718/google_analytics_dashboard/assets/104637982/4bdeed74-d758-4f82-8eab-bc832256921b" width = 500>
</div>

왼쪽 그래프를 보면 전체 검색 기능을 사용했을 때 나온 결과를 0번 클릭한 세션이 압도적으로 많은 것을 볼 수가 있음!  
오른쪽 그래프를 보면 x축이 한 세션내에서 몇번 전체 검색 기능 사용 횟수를 나타내고 y축은 클릭률을 나타냄. -> 한 세션 내에서 전체 검색 기능 사용 횟수가 늘어난다면 클릭률 또한 증가해야 하는데, 그렇지가 않음!  

즉, 어떻게 보면 전체 검색기능을 사용한 고객이 원하는 결과를 찾지 못해서 어떠한 결과도 클릭하지 않았고, 다른 검색어를 통해 찾으려고 했지만, 또 원하는 결과가 나오지 않아 클릭하지 않게됨!

## 몇 번째 결과물을 클릭했는가?

완전 검색 기능을 사용한 사람들이 몇 번째 결과물을 클릭했는지 살펴보자!  

<image src = "https://github.com/wbin0718/google_analytics_dashboard/assets/104637982/624debd8-81e0-4370-b7f2-9b8ff41915dc" width = 500>

위 그래프를 보면 분명 1,2,3 번째 결과를 많이 클릭하는 것으로 보임!  
하지만 좋은 검색 랭킹 알고리즘을 가지고 있다면 그 이상의 결과물을 클릭하는 건 적어야 하는 것이 맞지 않을까?  
하지만 3번째 이상 결과물을 클릭하는 경우도 마찬가지로 많음! -> 랭킹 알고리즘에 문제가 있고, 사용자들이 원하는 결과를 바로 얻을 수 없어 완전 검색 기능을 사용하지 않는 것이 아닐까?

## 완전 검색 사용자의 리텐션 분석

완전 검색 기능을 경험하고 1달 이내 같은 기능을 몇 번 다시 이용하는 지 살펴보자!

<div>
 <image src = "https://github.com/wbin0718/google_analytics_dashboard/assets/104637982/7ba43bf0-b227-4482-89b8-be9cd63fd038" width = 500>
 <image src = "https://github.com/wbin0718/google_analytics_dashboard/assets/104637982/9a991e9e-f6e0-4477-89f8-f6ffff7aff89" width = 500>
</div>

왼쪽 그래프를 보면 완전 검색 기능을 사용한 사용자가 처음 완전 검색 기능을 사용하고 한달 이내 몇 번을 더 사용하는 지를 나타내는 리텐션 차트임.  
리텐션이 급격히 떨어지는 것을 보임! -> 위 분석처럼 원하는 결과를 찾는 데 어려움을 겪어 사람들이 사용하지 않는 것 같음!  

오른쪽 그래프를 보면 자동 완성 검색 기능을 사용한 사용자가 처음 자동 완성 검색 기능을 사용하고 한달 이내 몇 번을 더 사용하는 지를 나타내는 리텐션 차트임.  
리텐션이 완전 검색과 달리 급격히 떨어지지 않고 완만히 감소하는 것으로 보임! -> 자동 완성 기능은 어느정도 사용자에게 도움을 주고 있어 활용하고 있지만, 완전 검색 기능은 도움이 되지 않아 사용하는 빈도가 적어지는 것을 알 수가 있음!

## 분석 결과

* 전체 세션 중 25%가 자동 완성 기능을 사용한다는 것은 사용자가 그 기능을 필요로 하는 것으로 볼 수 있음!
* 하지만 완전 검색 기능은 자동 완성보다 낮은 8%의 사용률을 보임.
* 완전 검색의 경우 원하는 결과가 상위 결과로 바로 나오지 않아 사람들의 검색 횟수, 클릭 횟수가 많아짐!
* 그 결과, 원하는 결과를 바로 찾지 못하는 것을 알게된 사용자들은 완전 검색 기능을 다시 사용하려고 하지 않음!
* 즉, 기능을 개선할 때 검색 기능을 먼저 개선하는 것은 괜찮으며 완전 검색 기능의 랭킹 알고리즘에 문제가 있음을 알려 이를 개선하는 방향을 제시할 수 있음!

# 2.3 A/B TEST 분석

## 문제 정의

yammer의 프로덕트팀은 publish라는 새로운 기능을 개발함!  
2014년 6월 1일 ~ 2014년 6월 30일 기간 동안 사용자들에게 서로 다른 화면을 보여주는 A/B Test를 진행함!

<image src = "https://github.com/wbin0718/google_analytics_dashboard/assets/104637982/a375cefc-3df2-4b05-94e4-21e14bd887a9" width = 500>

A/B Test를 진행한 결과 메시지 포스트 비율이 새로운 기능을 경험한 사용자가 기존 기능을 경험한 사용자보다 50% 정도 향상됨!  

그런데, 새로운 기능을 추가 했다고 메시지 포스팅 비율이 50%나 증가할 수 있을까? 너무 급격한 차이로 인해 A/B Test가 잘못된 것은 아닌지 의문점을 갖게됨!

## 가설 설정

* 통계적 검정을 잘 한것이 맞을까?
* A/B Test를 진행하고 확인한 KPI 지표가 적절한가?
* 샘플링이 잘 된 것이 맞을까?

## 로그인 지표 분석

* 새로운 기능을 통해 새로운 기능을 많이 사용하거나 적게 사용하는 것은 파악할 수 있지만, 이것이 진짜 yammer의 최종 사용자가 도달하기를 원하는 목표인가? -> yammer의 최종 목표는 사용자가 login을 하는 것!
* 단순히 메시지 포스팅 비율을 보는 것이 아닌 최종 목표인 로그인을 더 많이하는가?를 대답해볼 필요가 있음!

<image src = "https://github.com/wbin0718/google_analytics_dashboard/assets/104637982/fe737974-c1df-4dee-afad-fb51f0ae9807" width = 500>

* 위 그래프를 보면 test_group 즉 새로운 기능을 사용한 그룹이 로그인하는 비율이 더 높음! -> 여기서 쿼리를 한 사용자가 여러 세션을 하루에 접속했을 때로 구했음!, 단순히 하루에 여러번 로그인한 사용자로 인해 값이 높아졌을 수도 있음! -> 하루에 여러번 로그인한 사용자를 하나로 계산하자!

<image src = "https://github.com/wbin0718/google_analytics_dashboard/assets/104637982/12e0ea2a-728a-46eb-a290-c32ee2ad5923" width = 500>

* 하루에 여러 세션 로그인을 한 사용자를 하나로 집계하니 test_group, control_group 모두 로그인 비율이 낮아진 것을 볼 수가 있음!
* 하지만, 그래도 여전히 test_group의 로그인을 한 비율이 더 높은 것을 볼 수가 있음!

## 샘플링의 공정성

* 6월 한달동안 A/B Test를 진행했는데, 6월달 가입자는 다른 가입자들보다 로그인 및 메시지 포스팅 노출이 상대적으로 적을 수 밖에 없음!
* 월별 가입자별로 어떤 그룹으로 test가 진행되었는지 확인 해 볼 필요가 있음!

<image src = "https://github.com/wbin0718/google_analytics_dashboard/assets/104637982/962d9424-625e-4fbd-83db-2638f83e8cbf" width = 500>

* 위 그래프를 보면 재밌는 것을 발견할 수가 있음. -> 6월 가입자가 전부 control_group으로 속했다는 것!
* 즉 6월 가입자가 전부 control_group으로 속해 control_group의 로그인 비율, 메시지 포스팅 비율이 낮게 집계 되도록 한것일 수도 있음!

<div>
 <image src = "https://github.com/wbin0718/google_analytics_dashboard/assets/104637982/fcf40f7d-546c-4eef-ab75-ce22cdca1a4b" width = 500>
 <image src = "https://github.com/wbin0718/google_analytics_dashboard/assets/104637982/ccf00e32-0c94-45a4-96a6-50fbfe600a37" width = 500>
</div>

* 위 그래프를 보면 6월 가입자를 제외하고 다시 메시지 포스팅 비율, 로그인 비율을 살펴봄!
* 6월 가입자를 제외하니 control_group의 평균 값이 상승한 것을 볼 수가 있음! -> 6월 가입자가 control_group의 비율을 낮추고 있었음!
* 하지만, 6월 가입자를 제외해도 test_group의 로그인 비율 및 메시지 포스팅 비율이 높은 것을 볼 수가 있음!

따라서 A/B Test의 결과가 잘못된 것이 아닌 것으로 판단할 수 있음!

## 분석 결과

* A/B Test의 결과가 실험군과 대조군의 차이가 너무 많이나는 문제가 발생!
* 단순히 기능적 지표가 아닌 yammer의 최종 목표인 로그인 비율을 함께 살펴봄! -> 로그인 지표도 test group이 더 높은 평균을 가짐!
* 6월 가입자가 전부 control group으로 속하는 샘플링 문제가 발생함! -> 6월 가입자를 제외하고 로그인 지표, 메시지 포스팅 지표를 살펴봐도 test group의 평균이 더 높은 것을 볼 수가 있음!
* 따라서 A/B Test의 결과를 믿을 수 있다고 판단하게 됨!

# 분석하면서 느낀점

* 데이터 분석을 할 때 문제 정의가 가장 중요한 것을 깨달을 수 있었음! -> 문제 정의가 제대로 되지 않으면 분석을 하다가 헤매는 경우가 있음!, 어떤 분석을 해야할지 모르는 경우가 많음!
* 또한 문제가 정의되어도 단순히 데이터를 살펴보는 것은 좋지 않음! -> 내가 무엇을 먼저 봐야할지, 어떤 데이터를 봐야할지 알기가 어려움!
* 즉, 다양한 가설을 먼저 세워보고, 각 가설에 맞는 데이터를 찾고, 가설을 검증하는 것이 중요함!, 가설이 없다면 분석이 산으로 가는 것 같음!
* 데이터를 분석할 때 단순히 시각화도 좋지만 피벗 테이블 형식으로 만들어 모수의 개수 등을 보며 객관적인 분석이 필요한 것을 깨닫게 됨!








































