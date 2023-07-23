# DB🖤💎 ver. jsp & mvc2
> 의류 쇼핑몰 제작

![image](https://github.com/jeongjayun/jeongjayun.github.io/assets/116062065/4444af38-484a-45ff-8f33-47ac73d92faf)

## 목차
1. 프로젝트 개요
2. 사용 기술
3. 프로젝트 설계
4. 프로젝트 주요 기능
5. 개인 내용
6. 느낀점

## 1. 프로젝트 개요
- 프로젝트 소개 : 쇼핑몰 웹 사이트 구현
- 개발인원 : 4명
- 역할 : 데이터베이스, 장바구니 및 결제 페이지, 주문내역
- 개발기간 : 2023. 04. 10 - 2023. 04.21

## 2. 사용기술
### Front End
<img src="https://img.shields.io/badge/html5-E34F26?style=for-the-badge&logo=html5&logoColor=white"><img src="https://img.shields.io/badge/css-1572B6?style=for-the-badge&logo=css3&logoColor=white"><img src="https://img.shields.io/badge/javascript-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black"><img src="https://img.shields.io/badge/jquery-0769AD?style=for-the-badge&logo=jquery&logoColor=white">

### Back End
<img src="https://img.shields.io/badge/java-007396?style=for-the-badge&logo=java&logoColor=white"><img src="https://img.shields.io/badge/apache tomcat-F8DC75?style=for-the-badge&logo=apachetomcat&logoColor=white">

### DataBase
<img src="https://img.shields.io/badge/oracle-F80000?style=for-the-badge&logo=oracle&logoColor=white">

### IDE
<img src="https://img.shields.io/badge/eclipseide-2C2255?style=for-the-badge&logo=eclipseide&logoColor=white">

## 3. 프로젝트 설계
### 데이터베이스
[![image](https://github.com/jeongjayun/jeongjayun.github.io/assets/116062065/784d1efd-69d4-4f8c-9eff-6d0a56dcbc8e)](https://github.com/jeongjayun/jeongjayun.github.io/assets/116062065/784d1efd-69d4-4f8c-9eff-6d0a56dcbc8e)
![image](https://github.com/jeongjayun/DBjsp/assets/116062065/8392fbbb-da40-44a0-8afc-1f6f6eb58286)![image](https://github.com/jeongjayun/DBjsp/assets/116062065/a6a41bda-c20d-41c0-ac63-cc3a2d791887)![image](https://github.com/jeongjayun/DBjsp/assets/116062065/7f3b0de2-c4de-45db-a08d-86cf2a5e56c5)![image](https://github.com/jeongjayun/DBjsp/assets/116062065/37b1479a-d4e7-476b-b4e0-600c848a4f83)![image](https://github.com/jeongjayun/DBjsp/assets/116062065/91c49cb8-15fd-4cc3-ac64-d4f32b1ec8f5)![image](https://github.com/jeongjayun/DBjsp/assets/116062065/5fe460ad-d706-497e-af3f-0e454de40439)






## 4. 프로젝트 주요 기능
<details>
<summary> 주요 기능 및 실행화면 </summary>

  <div>
    
  - 메인 화면 및 로그인 하지 않은 상태

![vid![image](https://github.com/jeongjayun/DBjsp/assets/116062065/77158053-5813-4971-8795-4795c51bf29f)
eoplayback](https://github.com/jeongjayun/DBjsp/assets/116062065/7c9d308c-5c81-404b-a9c0-b7b748db3838)


  - 회원가입과 로그인(유효성 검사)

![videoplayback-2](https://github.com/jeongjayun/DBjsp/assets/116062065/91881dce-df24-493e-930f-b6995a1e1f21)

![image](https://github.com/jeongjayun/DBjsp/assets/116062065/de01bfd7-5acc-4bbc-967f-d31f766f399f)

  - sha256 비밀번호 인코딩되어 저장됨 

  - 상품 검색

![videoplayback-3](https://github.com/jeongjayun/DBjsp/assets/116062065/df037405-bfda-432b-a28e-79bd4043fd8e)

  - 마이페이지

![videoplayback-4](https://github.com/jeongjayun/DBjsp/assets/116062065/830f6abd-a56a-453a-b09d-8eab895e3524)

  - 게시판 이용

![videoplayback-5](https://github.com/jeongjayun/DBjsp/assets/116062065/9d46e98b-8a2d-499d-957a-c0b3ecae62f5)

  - 관리자 페이지

![videoplayback-6](https://github.com/jeongjayun/DBjsp/assets/116062065/26d4662d-6bc1-41a9-a912-e343b54c5e4f)

   
  </div>
  
</details>

## 5. 개인 내용
### 상세도
![image](https://github.com/jeongjayun/DBjsp/assets/116062065/d4e6266d-8cd4-4310-a7f8-61beb68c585c)

장바구니에서 결제페이지까지 코드를 작성했습니다.

### 트러블슈팅

```java
public int insertOrder(ArrayList<CartVO> listCart, String userid, int total) {
		int maxOrderNumber = 0;

		Connection conn = null;
		PreparedStatement pstmt = null;
		ResultSet rs;

		try {

			conn = DBManager.getConnection();
			String selectMaxOrderNumber = "select max(orderNumber) from orders";
			pstmt = conn.prepareStatement(selectMaxOrderNumber);
			rs = pstmt.executeQuery();

			if (rs.next()) {
				maxOrderNumber = rs.getInt(1);
			}

			pstmt.close();

			String insertOrder = "insert into orders(orderNumber,userid)
							values(" + "orders_seq.nextval,?)";
			pstmt = conn.prepareStatement(insertOrder);
			pstmt.setString(1, userid);
			pstmt.executeUpdate();

			for (CartVO cartVO : listCart) {
				insertOrderDetail(cartVO, maxOrderNumber + 1, total);
			}

			System.out.println("insertOrder (OrderDAO) 실행(+)");

		} catch (Exception e) {
			// TODO: handle exception
			System.out.println("insertOrder(OrderDAO) error : " + e);
		} finally {
			DBManager.close(conn, pstmt);
		}
		return maxOrderNumber + 1;
	}
```

1. orders, order_detail 테이블을 분리하여 주문페이지 와 주문내역 페이지에서 필요한 내용만 가져와 화면에 반환하고 합쳐야 할 경우에는 가상테이블 view join 쿼리를 이용해서 가져왔습니다. 그런 중 '주문하기' 기능 수행 중에 orders의 번호가 1부터 시작하는 것에 반해 orders_detail의 번호가 0부터 시작하는 에러를 마주쳐 임시방편으로 오류 수정하였습니다. 

## 6. 느낀점

java, mvc2 패턴을 이용한 쇼핑몰을 처음에 우리가 만들 수 있을까하고 반신반의 하는 마음이 컸습니다. java로 별찍기, 이클립스 콘솔창에 입력한 내용을 출력하기를 했던게 엊그제 같았는데 코드가 돌아가는 모습을 보니 무척 신기합니다.


 수업 시간에 MCV1, MVC2 패턴을 이용하여 각각 CRUD 기능을 구현해봤는데 첫 프로젝트 때와 비교하여도 MVC2 패턴이 팀원들과 협업 시 기능을 나누고 합치기 편리했습니다. 이번 프로젝트에서는 데이터베이스와 장바구니 및 결제 페이지 등을 맡아서 진행 했었는데 프로젝트 내에서 CRUD 기능을 충분히 연습하지 못한 것 같아서 아쉬움이 남습니다.


다음 프로젝트는 Spring을 이용하여 이번 프로젝트를 이어서 보다 보완할 수 있도록 노력하겠습니다.


