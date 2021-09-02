# Comento_HW4

### 1. 3차과제 까지 진행상황
  - 연도별 접속자 수
    - 주어진 연도와 일치하는 데이터 카운트
    - ![image](https://user-images.githubusercontent.com/71567319/130962989-5a892f26-0de6-4a93-a4b8-3c460e4ab356.png)
  - 월별 접속자 수
    - 주어진 연도, 월과 일치하는 데이터 카운트
    - ![image](https://user-images.githubusercontent.com/71567319/130962999-9e17b285-e248-4eb8-b9cb-70e1bde4efd8.png)
  - 일자별 접속자 수
    - 주어진 연도, 월, 일과 일치하는 데이터 카운트
    - ![image](https://user-images.githubusercontent.com/71567319/130963013-640d9290-9b67-495e-8a1c-4026e568cf78.png)
  - 평균 하루 로그인 수
    - 쿼리로 하루 평균 로그인 수 구해준다
    - ![image](https://user-images.githubusercontent.com/71567319/130963037-2910844b-7740-479b-9117-475a9952ce97.png)
  - 부서별 월별 로그인 수
    - 주어진 부서에서 주어진 년, 월별 로그인 수 카운트
    - ![image](https://user-images.githubusercontent.com/71567319/130963065-6245473c-4510-44ab-aad6-8a632f9479f4.png)

### 2. 4차 과제 추가 진행 상황 (휴일을 제외한 로그인 수)
  - 19년, 20년도 공휴일을 파악해 별도의 DB를 생성해 주었습니다. (해당 과제에선 19년, 20년 데이터만 사용)
    - ![image](https://user-images.githubusercontent.com/71567319/131850177-142c4fb0-1b98-4ed8-b43b-7c9618f84310.png)  ![image](https://user-images.githubusercontent.com/71567319/131849968-40c2576f-fa71-498b-998f-e0bf848a7bbd.png)
  - 총 11개의 데이터 중 20년 8월 15일을 제외하고 10번의 로그인이 있었다.
    - ![image](https://user-images.githubusercontent.com/71567319/131851030-e54b1881-4fcd-4626-837a-a836ba5c5577.png)  ![image](https://user-images.githubusercontent.com/71567319/131851360-58f4d246-0fc2-497b-ba76-5229cce3b1aa.png)
  - statisticMapper.xml
    ```
    <select id="selectHolidayLogin" parameterType="string" resultType="hashMap">
        select count(*) as totCnt
        from statistc.requestInfo ri
        where substring(ri.createDate, 1, 6) not in (select holi_date from holidayInfo)
    </select>
    ```
  - StatisticServiceImpl.java
    ```
    @Override
	public HashMap<String, Object> holidayLoginNum() {
		HashMap<String, Object> retVal = new HashMap<String,Object>();
        
		try {
            retVal = uMapper.selectHolidayLogin();
            retVal.put("is_success", true);
            
        }catch(Exception e) {
            retVal.put("totCnt", -999);
            retVal.put("is_success", false);
        }
        
        return retVal;
	}
    ```
  - settingTest.java
    ```
    @ResponseBody 
    @RequestMapping("/holiday")
    public Map<String, Object> holidaysqltest() throws Exception{ 
    	
    	return service.holidayLoginNum();
    	
    }
    ```

