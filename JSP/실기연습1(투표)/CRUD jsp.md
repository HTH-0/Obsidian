# 📄 index.jsp

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>

<style>
	:root{}
	html{}
	*{	 box-sizing:border-box;}
	body{padding:0;margin : 0;}
	ul{list-style:none;margin:0;padding:0;}
	a{text-decoration:none; color:black;}
	.wrapper{}
	.wrapper>header{height:80px;}
	.wrapper>nav{height:50px;}
	.wrapper>main{ height :calc(100vh - 80px - 50px - 80px);}
	.wrapper>main h2{
		text-align:center;
		font-size:1.8rem;
		font-weight:400;
	}
	.wrapper>main table{
		border:1px solid;
		border-collapse:collapse;
		min-width:500px;
		min-height:350px;
		margin: 0 auto;
	}
	.wrapper>main table th,
	.wrapper>main table td{
		min-width:80px !important;
		min-height:35px !important;
		border:1px solid;
		text-align:center;
	}
	.wrapper>main table th{
		background-color:lightgray;
	}
	.wrapper>footer{height:80px;}
</style>

</head>
<body>
	<%@page import="Utils.*,java.util.*" %>
	<%
	List<MemberDto> list = OracleDBUtils.getInstance().selectAllMember();
	%>
	<div class="wrapper">
		<%@include file="/layouts/Header.jsp" %>
		<%@include file="/layouts/Nav.jsp" %>
		
		<main>
			<h2>후보조회</h2>
			<table>
				<tr>
					<th>후보번호</th>
					<th>성명</th>
					<th>소속정당</th>
					<th>학력</th>
					<th>주민번호</th>
					<th>지역구</th>
					<th>대표전화</th>
				</tr>	
				<%
				for(MemberDto dto : list)
				{
				%>	
				<tr>
					<td><%=dto.getM_no() %></td>
					<td><%=dto.getM_name() %></td>
					<td><%=dto.getP_name() %></td>
				<%	
					String school = dto.getP_school();
					switch(school)
					{
						case "1":
							out.print("<td>고졸</td>");
							break;
						case "2":
							out.print("<td>학사</td>");
							break;
						case "3":
							out.print("<td>석사</td>");
							break;
						case "4":
							out.print("<td>박사</td>");
							break;
					}
				%>	
					<td><%=dto.getM_jumin() %></td>
					<td><%=dto.getM_city() %></td>
					<td><%=dto.getP_tel1()+"-"+dto.getP_tel2()+"-"+dto.getP_tel3() %></td>
				</tr>						
				<%	
				}
				%>
			</table>
		</main>
		
		<%@include file="/layouts/Footer.jsp" %>
	</div>
</body>
</html>
```

---

## 🧠 코드 설명

- `OracleDBUtils.getInstance().selectAllMember()`를 통해 DB에서 후보자 목록을 조회
    
- 조회된 후보자 리스트(`List<MemberDto>`)를 `for` 루프로 출력
    
- 학력 정보는 `1~4`로 구분된 숫자를 `고졸~박사`로 변환
    
- 출력 테이블에는 후보번호, 성명, 정당명, 학력, 주민번호, 지역구, 전화번호 표시
    
- `Header.jsp`, `Nav.jsp`, `Footer.jsp`를 인클루드하여 페이지 레이아웃 구성
    

---

# 📄 create.jsp

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
    
<%@page import="Utils.*" %>
<%
	request.setCharacterEncoding("UTF-8");
	response.setCharacterEncoding("UTF-8");
	response.setContentType("text/html; charset=UTF-8");

	String 	jumin = request.getParameter("v_jumin");
	String 	name = request.getParameter("v_name");
	String 	no = request.getParameter("m_no");
	String 	time = request.getParameter("v_time");
	String 	area = request.getParameter("v_area");
	String 	confirm = request.getParameter("v_confirm");

	VoteDto voteDto = new VoteDto(jumin,name,no,time,area,confirm);
	System.out.println("voteDto : " + voteDto);
%>  

<jsp:useBean id="voteDto2" class="Utils.VoteDto" scope="request" />
<jsp:setProperty name="voteDto2" property="*" />

<%
System.out.println("voteDto2 : " + voteDto2);
	int result = OracleDBUtils.getInstance().insertVote(voteDto2);
	
	if(result > 0){
		request.getRequestDispatcher("./read.jsp").forward(request,response);
	}else{
		out.println("<script>alert('투표 실패!다시하세요');location.href='./index.jsp'</script>");
	}
%>
```

---

## 🧠 코드 설명

- 투표 폼으로부터 전달받은 파라미터를 직접 받거나 액션 태그(`useBean`, `setProperty`)로 `VoteDto`에 저장
    
- `insertVote()` 메서드를 통해 `TBL_VOTE_202005` 테이블에 투표 데이터 저장
    
- 저장 성공 → `read.jsp`로 포워딩
    
- 실패 → 자바스크립트 alert 후 `index.jsp`로 리디렉션
    

---
