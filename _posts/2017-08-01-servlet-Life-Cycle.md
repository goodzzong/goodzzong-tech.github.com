---
layout: post
title: Servlet Life Cycle
name : goodzzong
---
#Servlet Life Cycle

서블릿은 자바파일이기 때문에 요청 할 때 마다 느릴꺼라고 생각하지만 실제로는 수행속도가 빠릅니다.
그 이유는 첫번째 요청시에는 내부적으로 컴파일일 이루어지지만 두번째 요청부터는 또 다시 요청이 이루어 질때
컴파일 하는 것이 아니라 한번 메모리에 로딩 되면 메모리에 계속 남아 추후에 서블릿을 호출할때 메모리에 이미 로딩된 서블릿을
불러오게 됩니다.


Instance 생성(서블릿 객체생성) -> init()(최초로 한번만 호출) -> doGet() or doPost() (요청될때마다 호출) ->
destroy() (톰캣 해제시 자원 해제)

1. 객체가 생성되면서 init() 단 한번 호출. 주로 초기화작업을 담당합니다.
2. 그 후에 서블릿을 요청할때 마다 반복적으로 doGet() or doPost()를 호출합니다.
3. 마지막으로 서블릿이 더 이상 서비스를 하지 않을 경우 destroy() 메소드를 호출. 
서블릿컨테이너(톰캣)가 종료 되거나 재가동 될때나 서블릿이 변경되어 다시 컴파일해서 클래스파일이 변경되는 경우입니다.


~~~
package com.jsplec.hw;

import java.io.IOException;

import javax.annotation.PostConstruct;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * Servlet implementation class HelloWorld
 */
@WebServlet("/HWorld")
public class HelloWorld extends HttpServlet {
	private static final long serialVersionUID = 1L;
    		
    /**
     * @see HttpServlet#HttpServlet()
     */
    public HelloWorld() {
        super();
        // TODO Auto-generated constructor stub
        System.out.println("Hello World!!!!!");
        
    }
    
	/**
	 * @see HttpServlet#doGet(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		// TODO Auto-generated method stub
		response.getWriter().append("Served at: ").append(request.getContextPath());
		System.out.println("doGet");
	}

	/**
	 * @see HttpServlet#doPost(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		// TODO Auto-generated method stub
		doGet(request, response);
	}
	
	@Override
	public void init() throws ServletException {
		// TODO Auto-generated method stub
		System.out.println("init");
	}
	@Override
	public void destroy() {
		// TODO Auto-generated method stub
		System.out.println("destroy");
	}
	
}
~~~

다음 예제를 실행 해보면 어느 시점에 호출 되는지 확인해 볼수 있습니다.