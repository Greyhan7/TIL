# [JSP & Servlet] 기본 문법 및 데이터베이스 연동

# 1. JSP&Servlet기본 개념

## 1) JSP (Java Server Page)란

### 정의 및 특징

- 정직인 HTML코드에 Java 코드를 넣어 동적으로 웹페이지를 구성할 수 있게 만드는 웹 어플리케이션 도구.
- 화면구성, 수정에 용이하지만 코드 및 소스 공개가 쉬워 중요한 정보를 구성하기에는 부적합하다.
- HTML 코드 속에서 <% %> 안에 자바 코드를 넣어 페이지를 구성할 수 있다.

- DAO와 VO를 생성하고 JSP 페이지를 통해 데이터베이스 값이 나오는 HTML페이지 만들기
    
    ```java
    package com.sist.vo;
    
    public class DeptVO { // 데이터베이스 값을 불러와서 호출하기 위한 VO 생성
    	private int dno;
    	private String dname;
    	private String dloc;
    	public int getDno() {
    		return dno;
    	}
    	public void setDno(int dno) {
    		this.dno = dno;
    	}
    	public String getDname() {
    		return dname;
    	}
    	public void setDname(String dname) {
    		this.dname = dname;
    	}
    	public String getDloc() {
    		return dloc;
    	}
    	public void setDloc(String dloc) {
    		this.dloc = dloc;
    	}
    	public DeptVO(int dno, String dname, String dloc) {
    		super();
    		this.dno = dno;
    		this.dname = dname;
    		this.dloc = dloc;
    	}
    	public DeptVO() {
    		super();
    	}
    	
    	
    }
    
    package com.sist.dao;
    
    import java.sql.Connection;
    import java.sql.DriverManager;
    import java.sql.ResultSet;
    import java.sql.Statement;
    import java.util.ArrayList;
    
    import com.sist.vo.DeptVO;
    
    public class DeptDAO { // 부서 목록을 반환하는 DAO 메소드
    	public ArrayList<DeptVO> search_all() {
    		ArrayList<DeptVO> list = new ArrayList<DeptVO>();
    		
    		String sql = "select dno, dname, dloc from dept";
    		
    		try {
    			Class.forName("oracle.jdbc.driver.OracleDriver");
    			Connection conn = DriverManager.getConnection(
    					"jdbc:oracle:thin:@172.30.1.14:1521:XE", 
    					"c##madang", "madang");
    			
    			Statement stmt = conn.createStatement();
    			ResultSet rs = stmt.executeQuery(sql);
    			
    			while(rs.next()) {
    				list.add(new DeptVO(rs.getInt(1), rs.getString(2), rs.getString(3)));
    			}
    			
    			rs.close();
    			stmt.close();
    			conn.close();
    			
    		}catch(Exception e) {
    			System.out.println("예외" + e.getMessage());}
    		return list;
    		
    	}
    }
    ```
    
    ```html
    <%@page import="com.sist.dao.DeptDAO"%>
    <%@page import="com.sist.vo.DeptVO"%>
    <%@page import="java.util.ArrayList"%>
    <%@ page language="java" contentType="text/html; charset=UTF-8"
        pageEncoding="UTF-8"%>
    <!DOCTYPE html>
    <html>
    <head>
    <meta charset="UTF-8">
    <title>Insert title here</title>
    </head>
    <body>
    	<%
    			
    			DeptDAO dao = new DeptDAO(); // 만들어놓은 dao를 생성
    			ArrayList<DeptVO> list = dao.search_all(); // vo를 매개변수로 갖는 list에 dao값 넣기
    			
    			// 생성된 dao와 vo를 통해 데이터베이스의 자료값 호출
    	%>
    	
    	<table border="1">	
    		<tr>
    			<td>부서번호</td>
    			<td>부서명</td>
    			<td>부서위치</td>
    		</tr>
    		
    		<%
    			for(DeptVO d:list){ // VO를 반복문 활용하여 생성하기
    				%>
    					<tr>
    						<td><%=d.getDno() %></td>
    						<td><%=d.getDname() %></td>
    						<td><%=d.getDloc() %></td>		
    					</tr>
    				<% 
    			}
    		
    		%>
    </body>
    </html>
    ```
    
    ![Untitled](%5BJSP%20&%20Servlet%5D%20%E1%84%80%E1%85%B5%E1%84%87%E1%85%A9%E1%86%AB%20%E1%84%86%E1%85%AE%E1%86%AB%E1%84%87%E1%85%A5%E1%86%B8%20%E1%84%86%E1%85%B5%E1%86%BE%20%E1%84%83%E1%85%A6%E1%84%8B%E1%85%B5%E1%84%90%E1%85%A5%E1%84%87%E1%85%A6%E1%84%8B%E1%85%B5%E1%84%89%E1%85%B3%20%E1%84%8B%E1%85%A7%E1%86%AB%E1%84%83%201ca7ed220f9e47a28605a49362fa21ff/Untitled.png)
    
- DAO와 VO를 생성하고 JSP 페이지를 통해 데이터베이스 값이 나오는 HTML페이지 만들기2
    
    ```java
    package com.sist.vo;
    
    import lombok.AllArgsConstructor;
    import lombok.Data;
    import lombok.EqualsAndHashCode;
    import lombok.Getter;
    import lombok.NoArgsConstructor;
    import lombok.Setter;
    import lombok.ToString;
    
    @Data
    @AllArgsConstructor
    @NoArgsConstructor
    public class BookVO { // 데이터베이스 값을 호출하는 VO생성 (rombok을 통해 간략히)
    	private int bookid;
    	private String bookname;
    	private String publisher;
    	private int price;
    	
    }
    
    package com.sist.dao;
    
    import java.sql.Connection;
    import java.sql.DriverManager;
    import java.sql.ResultSet;
    import java.sql.Statement;
    import java.util.ArrayList;
    
    import com.sist.vo.BookVO;
    
    public class BookDAO {
    	public ArrayList<BookVO> select_book() {
    		ArrayList<BookVO> list = new ArrayList<BookVO>();
    		String sql = "select * from book order by bookid";
    		
    		try {
    			Class.forName("oracle.jdbc.driver.OracleDriver");
    			Connection conn = DriverManager.getConnection(
    					"jdbc:oracle:thin:@172.30.1.14:1521:XE", 
    					"c##madang", "madang");
    			
    			Statement stmt = conn.createStatement();
    			ResultSet rs = stmt.executeQuery(sql);
    			
    			while(rs.next()) {
    				list.add(new BookVO(rs.getInt(1), rs.getString(2), rs.getString(3), rs.getInt(4)));
    			}
    			
    			if(rs != null) {
    			rs.close();
    			}
    			if(stmt != null) {
    				stmt.close();
    			}
    			if(conn != null) {
    				conn.close();
    			}
    			
    		}catch(Exception e) {
    			System.out.println("예외" + e.getMessage());}
    		return list;
    	}
    }
    ```
    
    ```html
    <%@page import="com.sist.vo.BookVO"%>
    <%@page import="java.util.ArrayList"%>
    <%@page import="com.sist.dao.BookDAO"%>
    <%@ page language="java" contentType="text/html; charset=UTF-8"
        pageEncoding="UTF-8"%>
    <!DOCTYPE html>
    <html>
    <head>
    <meta charset="UTF-8">
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@4.6.1/dist/css/bootstrap.min.css">
      <script src="https://cdn.jsdelivr.net/npm/jquery@3.6.0/dist/jquery.slim.min.js"></script>
      <script src="https://cdn.jsdelivr.net/npm/popper.js@1.16.1/dist/umd/popper.min.js"></script>
      <script src="https://cdn.jsdelivr.net/npm/bootstrap@4.6.1/dist/js/bootstrap.bundle.min.js"></script>
    <title>Insert title here</title>
    </head>
    <body>
    	<%
    		BookDAO dao = new BookDAO();
    		ArrayList<BookVO> list = dao.select_book();
    	%>
    
    	<table border="1" class="table table-dark table-hover">
    		<thead>
    		<tr>
    			<td>책 번호</td>
    			<td>책 이름</td>
    			<td>출판사</td>
    			<td>가격</td>
    		</tr>
    		</thead>
    		
    		<tbody>
    	
    		<%
    			for(BookVO book: list){
    				%>
    				<tr>
    						<td><%=book.getBookid()%></td>
    						<td><%=book.getBookname()%></td>
    						<td><%=book.getPublisher()%></td>
    						<td><%=book.getPrice()%></td>		
    					</tr>
    				
    				<%
    			}	
    		%>
    	</tbody>
    	</table>
    </body>
    </html>
    ```
    
    ![Untitled](%5BJSP%20&%20Servlet%5D%20%E1%84%80%E1%85%B5%E1%84%87%E1%85%A9%E1%86%AB%20%E1%84%86%E1%85%AE%E1%86%AB%E1%84%87%E1%85%A5%E1%86%B8%20%E1%84%86%E1%85%B5%E1%86%BE%20%E1%84%83%E1%85%A6%E1%84%8B%E1%85%B5%E1%84%90%E1%85%A5%E1%84%87%E1%85%A6%E1%84%8B%E1%85%B5%E1%84%89%E1%85%B3%20%E1%84%8B%E1%85%A7%E1%86%AB%E1%84%83%201ca7ed220f9e47a28605a49362fa21ff/Untitled%201.png)
    

## 2) Servlet이란

### 정의 및 특징

- 자바를 이용하여 웹 페이지를 동적으로 생성하는 서버측의 웹 프로그램. 웹을 만들기 위해 자바를 사용하는 기술.
- 클라이언트 측에서 특정 요청을 했을 때 그에 대한 결과를 서버 측에 전송하는 역할을 한다.
- MVC 패턴으로 웹 페이지를 만들 시 Controller 역할을 한다.

### 기본 구조

- HttpServletRequest
    - Http 프로토콜의 데이터를 전달하기 위한 객체.
    - 다음과 같은 메소드를 지닌다.
    - getparameter~ 를 통해 페이지에서 값을 전달 받을 수 있다.
    
    ![Untitled](%5BJSP%20&%20Servlet%5D%20%E1%84%80%E1%85%B5%E1%84%87%E1%85%A9%E1%86%AB%20%E1%84%86%E1%85%AE%E1%86%AB%E1%84%87%E1%85%A5%E1%86%B8%20%E1%84%86%E1%85%B5%E1%86%BE%20%E1%84%83%E1%85%A6%E1%84%8B%E1%85%B5%E1%84%90%E1%85%A5%E1%84%87%E1%85%A6%E1%84%8B%E1%85%B5%E1%84%89%E1%85%B3%20%E1%84%8B%E1%85%A7%E1%86%AB%E1%84%83%201ca7ed220f9e47a28605a49362fa21ff/Untitled%202.png)
    

- 데이터베이스와 연동하여 서블릿을 활용하여 데이터베이스 값을 추가하는 HTML 페이지 만들기 (서블릿의 Post 방식 사용)
    
    ```html
    <!DOCTYPE html>
    <html>
    <head>
    <meta charset="UTF-8">
    <title>Insert title here</title>
    </head>
    <body>
    	<h2>회원등록</h2>
    	<form action="insertMemberOK" method="post">
    		아이디 : <input type="text" name="id"><br>
    		패스워드 : <input type="password" name="password"><br>
    		이름 : <input type="text" name="name"><br>
    		<input type="submit" value="등록">
    	
    	</form>
    
    </body>
    </html>
    ```
    
    ```java
    package com.sist.vo;
    
    import lombok.AllArgsConstructor;
    import lombok.Data;
    import lombok.NoArgsConstructor;
    
    @Data
    @AllArgsConstructor
    @NoArgsConstructor
    
    public class MemberVO {
     private String id;
     private String password;
     private String name;
    }
    
    package com.sist.dao;
    
    import java.sql.Connection;
    import java.sql.DriverManager;
    import java.sql.PreparedStatement;
    
    import com.sist.vo.MemberVO;
    
    public class MemberDAO {
    
    	public int insertMember(MemberVO vo) {
    		int re = 0;
    		try {		
    			String sql = "insert into member(id, pwd, name) values(?,?,?)";
    			
    			Class.forName("oracle.jdbc.driver.OracleDriver");
    			Connection conn = DriverManager.getConnection(
    					"jdbc:oracle:thin:@172.30.1.14:1521:XE", 
    					"c##madang", "madang");
    			
    			PreparedStatement pstmt = conn.prepareStatement(sql);
    			pstmt.setString(1, vo.getId());
    			pstmt.setString(2, vo.getPassword());
    			pstmt.setString(3, vo.getName());
    			
    			re = pstmt.executeUpdate();
    			
    			if(pstmt != null) {
    				pstmt.close();
    			}
    			if(conn != null) {
    				conn.close();
    			}
    			
    		}catch(Exception e) {
    			System.out.println("예외" + e.getMessage());}
    				
    		
    		return re;
    	}
    	
    }
    
    package com.sist.servlet;
    
    import java.io.IOException;
    import java.io.PrintWriter;
    
    import javax.servlet.ServletException;
    import javax.servlet.annotation.WebServlet;
    import javax.servlet.http.HttpServlet;
    import javax.servlet.http.HttpServletRequest;
    import javax.servlet.http.HttpServletResponse;
    
    import com.sist.dao.MemberDAO;
    import com.sist.vo.MemberVO;
    
    /**
     * Servlet implementation class insertMemberOK
     */
    @WebServlet("/insertMemberOK")
    public class insertMemberOK extends HttpServlet {
    	private static final long serialVersionUID = 1L;
           
        /**
         * @see HttpServlet#HttpServlet()
         */
        public insertMemberOK() {
            super();
            // TODO Auto-generated constructor stub
        }
    
    	/**
    	 * @see HttpServlet#doGet(HttpServletRequest request, HttpServletResponse response)
    	 */
    	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
    		// TODO Auto-generated method stub
    		response.getWriter().append("Served at: ").append(request.getContextPath());
    	}
    
    	/**
    	 * @see HttpServlet#doPost(HttpServletRequest request, HttpServletResponse response)
    	 */
    	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
    		request.setCharacterEncoding("UTF-8"); // 한글 입력 가능하게 하도록 인코딩 변경
    		String id = request.getParameter("id");
    		String password = request.getParameter("password");
    		String name = request.getParameter("name");
    		// HTTP에서 입력하여 post한 값을 객체로 만들기
    		
    		String url = request.getRequestURL().toString();
    		String uri = request.getRequestURI().toString();
    		String ip = request.getRemoteAddr().toString();
    		
    		MemberVO vo = new MemberVO(id, password, name);
    		MemberDAO dao = new MemberDAO();
    		int re = dao.insertMember(vo); // vo와 dao를 활용하여 입력받은 값을 데이터베이스에 등록
    		
    		response.setContentType("text/html; charset=utf-8");
    		PrintWriter out = response.getWriter(); // 등록시 표기될 화면 정하기
    		
    		
    		if(re>0) {
    			out.print("<h2>회원 정보를 등록하였습니다</h2>");
    		}
    		else {
    			out.print("<h2>회원 정보 등록을 실패하였습니다</h2>");
    		}
    		out.print("URL:" + url + "<br>");
    		out.print("URI:" + uri + "<br>");
    		out.print("IP:" + ip + "<br>");
    		
    		out.print("<a href='insertMember.html'>다시 등록하기</a>");
    	}
    
    }
    ```
    

- HttpServlet
    - servlet을 만들기 위해 반드시 상속해야 하는 클래스.
    - 주요 메소드
        - void init() : servlet이 처음으로 만들어질 때 호출하는 메소드.
        - void service() : 클라이언트 요청이 있을 때마다 호출되는 메소드.
        - void destroy() : servlet 객체가 사라질 때 호출되는 메소드.
        - void doGet(), void doPost() : form 형태에 따라 데이터를 받는 메소드.
- HttpServletResponse
    - 클라이언트가 요청한 정보를 처리하고 재응답하기 위한 정보를 담고 있는 클래스.
- HttpSession
    - 클라이언트가 세션의 정보를 저장하고 세션 기능을 유지하기 위해 제공되는 클래스.

### 작동과정

![Untitled](%5BJSP%20&%20Servlet%5D%20%E1%84%80%E1%85%B5%E1%84%87%E1%85%A9%E1%86%AB%20%E1%84%86%E1%85%AE%E1%86%AB%E1%84%87%E1%85%A5%E1%86%B8%20%E1%84%86%E1%85%B5%E1%86%BE%20%E1%84%83%E1%85%A6%E1%84%8B%E1%85%B5%E1%84%90%E1%85%A5%E1%84%87%E1%85%A6%E1%84%8B%E1%85%B5%E1%84%89%E1%85%B3%20%E1%84%8B%E1%85%A7%E1%86%AB%E1%84%83%201ca7ed220f9e47a28605a49362fa21ff/Untitled%203.png)

![Untitled](%5BJSP%20&%20Servlet%5D%20%E1%84%80%E1%85%B5%E1%84%87%E1%85%A9%E1%86%AB%20%E1%84%86%E1%85%AE%E1%86%AB%E1%84%87%E1%85%A5%E1%86%B8%20%E1%84%86%E1%85%B5%E1%86%BE%20%E1%84%83%E1%85%A6%E1%84%8B%E1%85%B5%E1%84%90%E1%85%A5%E1%84%87%E1%85%A6%E1%84%8B%E1%85%B5%E1%84%89%E1%85%B3%20%E1%84%8B%E1%85%A7%E1%86%AB%E1%84%83%201ca7ed220f9e47a28605a49362fa21ff/Untitled%204.png)

- 클라이언트 측에서 URL을 통해 접속하면 HTTP Request에서 Servlet Container로 전송.
- Servlet Container가 HttpServlerRequest, HttpServlerResponse 객체를 생성.
- 요청한 URL이 어느 Servlet에 대한 요청인지 찾는다.
- init() 메소드를 통해 제일 우선적으로 실행하는 명령을 실시한다.
- 해당 Servlet에서 service 메소드 호출 후 클라이언트의 GET,POST 여부에 따라 doGet() 혹은 doPost()를 호출한다.
- doGet() 혹은 doPost()를 통해 동적 페이지를 생성하고 HttpServletResponse 객체에 응답을 보낸다.

# 2. JSP의 기초문법

## 1) 스크립트 요소

- 표현식 <%= %>
    - 변수를 출력하거나 메소드 결과 값을 출력할 수 있다.
    - 세미콜론을 쓰지 않는다.
- 선언문 <%! %>
    - 변수를 정의하거나 메소드를 정의할 수 있다.
- 스크립트릿 <% %>
    - 자바 코드를 입력할 수 있다.
- 주석
    - <% // %>, <% */ %>
    - 자바 문법에서 쓰던 형태의 주석을 그대로 사용.

## 2) 지시자(directive)

- JSP 실행될 때 필요한 정보를 JSP 컨테이너에게 알리는 역할을 가진 태그.
- 대표적으로 page와 include로 나뉜다.
    - page 지시자 <@page …%> : JSP 페이지에 대한 정보를 설정하는 태그.
    - include 지시자 <@include …%> : JSP 페이지의 특정 영역에 외부 파일 내용을 포함시키는 태그.

### page 지시자

- JSP 페이지 최상단에서 현재 JSP 페이지에 대한 정보를 설정한다.
- 인코딩이나 에러 페이지 등을 설정할 수 있다.
- <@ page속성1=”값1”%> 의 형태로 나타냄.

![Untitled](%5BJSP%20&%20Servlet%5D%20%E1%84%80%E1%85%B5%E1%84%87%E1%85%A9%E1%86%AB%20%E1%84%86%E1%85%AE%E1%86%AB%E1%84%87%E1%85%A5%E1%86%B8%20%E1%84%86%E1%85%B5%E1%86%BE%20%E1%84%83%E1%85%A6%E1%84%8B%E1%85%B5%E1%84%90%E1%85%A5%E1%84%87%E1%85%A6%E1%84%8B%E1%85%B5%E1%84%89%E1%85%B3%20%E1%84%8B%E1%85%A7%E1%86%AB%E1%84%83%201ca7ed220f9e47a28605a49362fa21ff/Untitled%205.png)

### include 지시자

- JSP 페이지의 특정 영역에 외부 파일 내용을 포함시킨다.
- HTML이나 텍스트 파일 등을 JSP에 넣을 수 있다.
- <%@ include file=”파일명”> 형태로 나타낸다.

## 3) 액션태그

- 서버나 클라이언트에게 어떤 기능을 하도록 명령하는 태그.
- JSP 페이지에서 페이지와 페이지 사이를 제어하고, 다른 페이지 실행결과 내용을 현재 페이지에 포함시킨다.

![Untitled](%5BJSP%20&%20Servlet%5D%20%E1%84%80%E1%85%B5%E1%84%87%E1%85%A9%E1%86%AB%20%E1%84%86%E1%85%AE%E1%86%AB%E1%84%87%E1%85%A5%E1%86%B8%20%E1%84%86%E1%85%B5%E1%86%BE%20%E1%84%83%E1%85%A6%E1%84%8B%E1%85%B5%E1%84%90%E1%85%A5%E1%84%87%E1%85%A6%E1%84%8B%E1%85%B5%E1%84%89%E1%85%B3%20%E1%84%8B%E1%85%A7%E1%86%AB%E1%84%83%201ca7ed220f9e47a28605a49362fa21ff/Untitled%206.png)

- 주요 태그들
    - forward : 다른 곳으로 보내기 위한 태그. response.sendReDirect(””)와 같은 기능을 하지만 주소가 드러나지 않는다.
    - include : include 지시자와 마찬가지로 다른 문서를 포함시킨다.
    - useBean : 자바 객체를 불러올 수 있다.
    
    ![위의 VO를 활용한 set, get과 밑의 useBean과 set,get 액션태그를 활용하면 같은 기능을 한다](%5BJSP%20&%20Servlet%5D%20%E1%84%80%E1%85%B5%E1%84%87%E1%85%A9%E1%86%AB%20%E1%84%86%E1%85%AE%E1%86%AB%E1%84%87%E1%85%A5%E1%86%B8%20%E1%84%86%E1%85%B5%E1%86%BE%20%E1%84%83%E1%85%A6%E1%84%8B%E1%85%B5%E1%84%90%E1%85%A5%E1%84%87%E1%85%A6%E1%84%8B%E1%85%B5%E1%84%89%E1%85%B3%20%E1%84%8B%E1%85%A7%E1%86%AB%E1%84%83%201ca7ed220f9e47a28605a49362fa21ff/Untitled%207.png)
    
    위의 VO를 활용한 set, get과 밑의 useBean과 set,get 액션태그를 활용하면 같은 기능을 한다
    
    - param : <jsp:forward>나 <jsp:include>의 자식 노드로 사용할 때 값을 전달한다.
    
    ![jsp:forward에서 param으로 자식 노드 생성](%5BJSP%20&%20Servlet%5D%20%E1%84%80%E1%85%B5%E1%84%87%E1%85%A9%E1%86%AB%20%E1%84%86%E1%85%AE%E1%86%AB%E1%84%87%E1%85%A5%E1%86%B8%20%E1%84%86%E1%85%B5%E1%86%BE%20%E1%84%83%E1%85%A6%E1%84%8B%E1%85%B5%E1%84%90%E1%85%A5%E1%84%87%E1%85%A6%E1%84%8B%E1%85%B5%E1%84%89%E1%85%B3%20%E1%84%8B%E1%85%A7%E1%86%AB%E1%84%83%201ca7ed220f9e47a28605a49362fa21ff/Untitled%208.png)
    
    jsp:forward에서 param으로 자식 노드 생성
    
    ![forward된 페이지에서 노드를 활용할 수 있음](%5BJSP%20&%20Servlet%5D%20%E1%84%80%E1%85%B5%E1%84%87%E1%85%A9%E1%86%AB%20%E1%84%86%E1%85%AE%E1%86%AB%E1%84%87%E1%85%A5%E1%86%B8%20%E1%84%86%E1%85%B5%E1%86%BE%20%E1%84%83%E1%85%A6%E1%84%8B%E1%85%B5%E1%84%90%E1%85%A5%E1%84%87%E1%85%A6%E1%84%8B%E1%85%B5%E1%84%89%E1%85%B3%20%E1%84%8B%E1%85%A7%E1%86%AB%E1%84%83%201ca7ed220f9e47a28605a49362fa21ff/Untitled%209.png)
    
    forward된 페이지에서 노드를 활용할 수 있음
    
    - DAO와 VO를 생성 후 액션 태그 useBean을 활용하여 데이터베이스에 값 입력하기
        
        ```html
        <!DOCTYPE html>
        <html>
        <head>
        <meta charset="UTF-8">
        <title>Insert title here</title>
        </head>
        <body>
        	<form action="insert_dept2.jsp" method="post">
        		부서번호 : <input type="number" name="dno">
        		부서명 : <input type="text" name="dname">
        		부서장소 : <input type="text" name="dloc">
        		
        		<input type="submit" value="등록">
        		<input type="reset" value="취소">
        	</form>
        
        </body>
        </html>
        ```
        
        ```html
        <%@page import="com.sist.dao.DeptDAO"%>
        <%@page import="com.sist.vo.DeptVO"%>
        <%@ page language="java" contentType="text/html; charset=UTF-8"
            pageEncoding="UTF-8"%>
        <!DOCTYPE html>
        <html>
        <head>
        <meta charset="UTF-8">
        <title>Insert title here</title>
        </head>
        <body>
        	<%
        		request.setCharacterEncoding("UTF-8");
        	/*int dno = Integer.parseInt(request.getParameter("dno"));
        	String dname = request.getParameter("dname");
        	String dloc = request.getParameter("dloc");
        	
        	DeptVO vo = new DeptVO(dno, dname, dloc);
        	DeptDAO dao = new DeptDAO();
        	int re = dao.insertDept(vo);*/
        		
        	%>
        	
        	위의 코드를 밑의 useBean을 통해 간략히 표현할 수 있다
        	<jsp:useBean id="vo" class="com.sist.vo.DeptVO"/>
        	<jsp:setProperty property="*" name="vo"/>
        	<jsp:useBean id="dao" class="com.sist.dao.DeptDAO"/>
        	
        	<%
        		int re = dao.insertDept(vo);
        		out.print(re);
        	%>
        	
        	
        	<h2>등록되었습니다</h2>
        
        </body>
        </html>
        ```
        

## 4) 내장객체

[](https://velog.io/@ansalstmd/JSP5.-%EB%82%B4%EC%9E%A5-%EA%B0%9D%EC%B2%B4)

- JSP 페이지를 작성할 때 특별한 기능을 제공하는 객체.
- 따로 선언하지 않더라도 사용할 수 있다.

![Untitled](%5BJSP%20&%20Servlet%5D%20%E1%84%80%E1%85%B5%E1%84%87%E1%85%A9%E1%86%AB%20%E1%84%86%E1%85%AE%E1%86%AB%E1%84%87%E1%85%A5%E1%86%B8%20%E1%84%86%E1%85%B5%E1%86%BE%20%E1%84%83%E1%85%A6%E1%84%8B%E1%85%B5%E1%84%90%E1%85%A5%E1%84%87%E1%85%A6%E1%84%8B%E1%85%B5%E1%84%89%E1%85%B3%20%E1%84%8B%E1%85%A7%E1%86%AB%E1%84%83%201ca7ed220f9e47a28605a49362fa21ff/Untitled%2010.png)

### 01. request

- 클라이언트에서 서버로 전달되는 정보를 처리하기 위해 사용하는 가장 대표적인 내장객체.

![request 내장 객체의 메소드들](Dev%2091f791871a3b4553a4a0dce1040f7b6b/%5B22%2008%2008%20~%2023%2003%2007%5D%20%E1%84%8A%E1%85%A1%E1%86%BC%E1%84%8B%E1%85%AD%E1%86%BC%E1%84%80%E1%85%A1%E1%86%BC%E1%84%87%E1%85%AE%E1%86%A8%E1%84%80%E1%85%AD%E1%84%8B%E1%85%B2%E1%86%A8%E1%84%89%E1%85%A6%E1%86%AB%E1%84%90%E1%85%A5%20094ee42b99b6460ca4538add5c6b6d4d/%5B10%2019%5D%20JSP%20(%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%B8%E1%84%90%E1%85%B3%20%E1%84%8B%E1%85%AD%E1%84%89%E1%85%A9%20&%20%E1%84%8B%E1%85%A2%E1%86%A8%E1%84%89%E1%85%A7%E1%86%AB%E1%84%90%E1%85%A2%E1%84%80%E1%85%B3%20&%20%E1%84%82%E1%85%A2%E1%84%8C%E1%85%A1%E1%86%BC%E1%84%80%E1%85%A2%206a523a7f43644e719243247ef1c7d85d/Untitled.png)

request 내장 객체의 메소드들

![Untitled](%5BJSP%20&%20Servlet%5D%20%E1%84%80%E1%85%B5%E1%84%87%E1%85%A9%E1%86%AB%20%E1%84%86%E1%85%AE%E1%86%AB%E1%84%87%E1%85%A5%E1%86%B8%20%E1%84%86%E1%85%B5%E1%86%BE%20%E1%84%83%E1%85%A6%E1%84%8B%E1%85%B5%E1%84%90%E1%85%A5%E1%84%87%E1%85%A6%E1%84%8B%E1%85%B5%E1%84%89%E1%85%B3%20%E1%84%8B%E1%85%A7%E1%86%AB%E1%84%83%201ca7ed220f9e47a28605a49362fa21ff/Untitled%2011.png)

![Untitled](%5BJSP%20&%20Servlet%5D%20%E1%84%80%E1%85%B5%E1%84%87%E1%85%A9%E1%86%AB%20%E1%84%86%E1%85%AE%E1%86%AB%E1%84%87%E1%85%A5%E1%86%B8%20%E1%84%86%E1%85%B5%E1%86%BE%20%E1%84%83%E1%85%A6%E1%84%8B%E1%85%B5%E1%84%90%E1%85%A5%E1%84%87%E1%85%A6%E1%84%8B%E1%85%B5%E1%84%89%E1%85%B3%20%E1%84%8B%E1%85%A7%E1%86%AB%E1%84%83%201ca7ed220f9e47a28605a49362fa21ff/Untitled%2012.png)

| 메소드명 | 기능 | 예시 |
| --- | --- | --- |
| getParameterNames() | 현재 요청에 포함된 매개변수의 이름을 리턴 |  |
| getParameter(name) | 문자열 name에 매칭되는 value를 리턴 |  |
| getParameterValues(name) | 문자열 name에 매칭되는 value를 배열 형태로 리턴 |  |
| getCookies() | 모든 쿠키 값을 javax.servlet.http.Cookie 배열 형태로 리턴 |  |
| getRemoteAddr() | 클라이언트의 IP주소를 리턴 |  |
| setCharacterEncoding() | JSP로 전달되는 내용을 지정한 캐릭터셋으로 변환시킨다. 한글 출력하기 위해서 사용한다. | request.setCaracterEncoding(”UTF-8”) |

### 02. response

- 사용자 요청(request)을 처리하고 해당 응답을 다른 페이지로 전달하는 내장객체.

| 메소드 | 기능 | 예시 |
| --- | --- | --- |
| setContentType(type) | contentType을 설정한다. | response.setContentType(”text/html; charset=utf-8”) |
| sendRedirect | 클라이언트 요청을 다른 페이지로 보낸다. | response.sendRedirect(”/main.html”) |

### 03. session

- 페이지를 이동할 때마다 http 프로토콜 상에 특정한 정보를 저장하기 위해 사용하는 내장객체.

![Untitled](%5BJSP%20&%20Servlet%5D%20%E1%84%80%E1%85%B5%E1%84%87%E1%85%A9%E1%86%AB%20%E1%84%86%E1%85%AE%E1%86%AB%E1%84%87%E1%85%A5%E1%86%B8%20%E1%84%86%E1%85%B5%E1%86%BE%20%E1%84%83%E1%85%A6%E1%84%8B%E1%85%B5%E1%84%90%E1%85%A5%E1%84%87%E1%85%A6%E1%84%8B%E1%85%B5%E1%84%89%E1%85%B3%20%E1%84%8B%E1%85%A7%E1%86%AB%E1%84%83%201ca7ed220f9e47a28605a49362fa21ff/Untitled%2013.png)

- 로그인과 로그아웃 시 활용한다.
- 로그아웃하거나 브라우저를 닫기 전까지 정보가 유지된다.
- session의 값은 문자, 숫자, 배열 등 모든 자료형을 포함시킬 수 있다.
- get이나 $(값 이름)을 통해 값을 가져올 수 있다.

### 04. application

- 웹 어플리케이션 전체를 관리하는 객체. 각 servlet이나 JSP에서 공유하고자 하는 각종 정보를 설정, 참조할 수 있다.
- 서버를 종료하기 전까지 정보가 유지된다.

| 메소드 | 기능 | 예시 |
| --- | --- | --- |
| log(message, exception) | 예외를 포함한 message의 내용을 로그 파일로 기록한다. |  |
| getAttribute(String name) | 문자열 name에 해당하는 속성 값을 Object 형태로 반환한다. |  |
| getAttributeNames() | application 객체에 저장된 속성들의 이름을 열거 형태로 가져온다. |  |
| setAttribute(String name, Object value) | 문자열 name의 이름으로 Object형 데이터를 저장한다. |  |
| removeAttribute(String name) | 문자열 name에 해당하는 속성을 삭제한다. |  |

## 5) 자바빈즈 ****

- JSP의 액션 태그로 접근할 수 있는 자바 클래스. 값을 설정하는 setter와 추출하는 getter로 이루어져 있다.
- jsp 페이지가 복잡한 자바 코드로만 구성되는 걸 피하고, html 같은 쉽고 간단한 코드로 구성되도록 하여 프로그래머와 디자이너가 더욱 쉽게 협업할 수 있도록 한다.

## 6) JSTL(JSP Standard Tag Library)

- jsp 페이지에서 일반적인 핵심 기능을 캡슐화하여 제공하는 태그 라이브러리.
- jstl.jar, standard.jar 파일을 WEB-INF/lib 안에 넣고 사용한다.
    - 아래 링크에서 다운로드 가능.
    
    [Apache Taglibs - Apache Standard Taglib: JSP[tm] Standard Tag Library (JSTL) implementations](https://tomcat.apache.org/taglibs/standard/)
    
- 사용하기 전 태그 라이브러리를 선언해야 한다.

| 태그 라이브러리 | 선언문 |
| --- | --- |
| core | <%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %> |
| XML | <%@ taglib prefix="x" uri="http://java.sun.com/jsp/jstl/xml" %> |
| Database | <%@ taglib prefix="fmt" uri="http://java.sun.com/jsp/jstl/fmt" %> |
| Functions | <%@ taglib prefix="sql" uri="http://java.sun.com/jsp/jstl/sql" %> |
| |18N | <%@ taglib prefix="fn" uri="http://java.sun.com/jsp/jstl/functions" %> |

### 태그 목록

- <c:out>
    - 출력문을 만드는 태그.
    
    ```html
    <c:out value="출력값"></c:out>
    ```
    
- <c:set>
    - 변수를 설정할 때 사용하는 태그.
    
    ```html
    <c:set var="변수명" value="값">
    ${변수명} or ${pageScope.변수명} 으로 사용
    
    객체가 따로 있다면 property를 설정할 수 있다.
    <c:set target="${객체명}" property="프로퍼티명" value="값">
    ```
    
- <c:if>
    - 조건식을 설정할 때 사용하는 태그.
    
    ```html
    <c:if test="조건식" var="변수명">
    ${변수명} : true 혹은 false로 출력됨.
    ```
    
- <c:forEach>
    - 반복문을 설정할 때 사용하는 태그.
    
    ```html
    <c:forEach var="변수명" items="목록데이터" begin="시작인덱스" end="종료인덱스"></c:forEach>
    
    예를 들어
    <% pageContext.setAttribute("numList", new String[]{"1", "2", "3"}); %>
    <ul>
    	<c:forEach var="num" items="${numList}">
    		<li>${num}</li>
    	</c:forEach>
    </ul>
    으로 1,2,3을 li 형태로 만들 수 있다.
    ```
    

- <c:redirect>
    - 클라이언트 페이지를 다른 곳으로 이동시킬 때 쓰는 태그.
    
    ```html
    <c:redirect url="url주소"/>
    ```
    

# 3. JSP&Servlet 활용하기

## 1) 데이터베이스 연동하기

### Connection Pool 활용하기

[Apache Tomcat 9](https://tomcat.apache.org/tomcat-9.0-doc/jndi-datasource-examples-howto.html#Oracle_8i,_9i_&_10g)

![Untitled](%5BJSP%20&%20Servlet%5D%20%E1%84%80%E1%85%B5%E1%84%87%E1%85%A9%E1%86%AB%20%E1%84%86%E1%85%AE%E1%86%AB%E1%84%87%E1%85%A5%E1%86%B8%20%E1%84%86%E1%85%B5%E1%86%BE%20%E1%84%83%E1%85%A6%E1%84%8B%E1%85%B5%E1%84%90%E1%85%A5%E1%84%87%E1%85%A6%E1%84%8B%E1%85%B5%E1%84%89%E1%85%B3%20%E1%84%8B%E1%85%A7%E1%86%AB%E1%84%83%201ca7ed220f9e47a28605a49362fa21ff/Untitled%2014.png)

- 데이터베이스와 연결된 커넥션을 미리 만들고 이를 pool로 관리하는 기법. 필요할 때마다 커넥션을 이용하기 때문에 빠르게 DB에 접속할 수 있다.
- DB의 부하를 줄이고 시간을 단축시키기 때문에 효율적으로 DB를 이용할 수 있다.

![Untitled](%5BJSP%20&%20Servlet%5D%20%E1%84%80%E1%85%B5%E1%84%87%E1%85%A9%E1%86%AB%20%E1%84%86%E1%85%AE%E1%86%AB%E1%84%87%E1%85%A5%E1%86%B8%20%E1%84%86%E1%85%B5%E1%86%BE%20%E1%84%83%E1%85%A6%E1%84%8B%E1%85%B5%E1%84%90%E1%85%A5%E1%84%87%E1%85%A6%E1%84%8B%E1%85%B5%E1%84%89%E1%85%B3%20%E1%84%8B%E1%85%A7%E1%86%AB%E1%84%83%201ca7ed220f9e47a28605a49362fa21ff/Untitled%2015.png)

## 2) 파일 업로드하기

- cos.jar 설치
- 파일 업로드 하는 form 태그에 enctype=”multipart/form-data”를 입력하여 파일의 인코딩 방식을 설정한다.
- form을 통해 데이터를 전달받을 때 request 객체가 아니라 multipartRequest 객체로 전달받는다.

## 3) 계층형 게시판 만들기

## 4) 페이징 처리하기

# 4. MVC패턴

- JSP 기본 개념
    - JSP와 Servlet이란
    - JSP와 웹
    
- JSP 기초 문법
    - 스크립트 요소
    - 지시자
    - 액션태그
    - 내장객체
    - 자바빈즈
    - JSTL

- JSP & Servlet 활용
    - 커넥션 풀
    - 파일 업로드하기
    - 계층형 게시판 만들기
    - 페이징 처리하기
    
- MVC 패턴으로 페이지 만들기
