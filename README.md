Manor Pharmacy  - Java Website Design Project 
            
            The aim of the project is to develop an open-source, platform independent and scalable web-based application which can suggest dietary plan to patients with specific BMIs.

Aims and Objectives : (Website Design Model)

This is the plan for designing the website and changes can be made depending on changing scenarios.

	Register User
	Login
	Personal Details
	Calculate calories
	Meal Plan and Type
	Plan Meal plan and Exercise for patient
	User selects  (Yes)  --- Recommendation
	User selects (NO) --- Plan an alternative diet plan and Exercise

Flowchart :
The user will be created first and then can log in to the system. After this, personal details will be taken or stored and taking those the calories will be calculated. The current meal plan and type will also be considered and based on that a meal plan and exercise planed out. User can either approve or reject the meal plan. If the user selects yes it will give the recommendations else if the user selects no it will plan an alternative diet plan and exercise for the patient.


 






Database Schema in HTML Format: 

 ![image](https://github.com/user-attachments/assets/d9822cf3-4db6-46c7-970d-f2fd56042e16)


1.	   Admin Login:
	
package action;

import java.io.IOException;
import java.io.PrintWriter;
import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;

/**
 *
 * @author IBN5
 */
public class admin_login extends HttpServlet {

    /**
     * Processes requests for both HTTP
     * <code>GET</code> and
     * <code>POST</code> methods.
     *
     * @param request servlet request
     * @param response servlet response
     * @throws ServletException if a servlet-specific error occurs
     * @throws IOException if an I/O error occurs
     */
    protected void processRequest(HttpServletRequest request, HttpServletResponse response)
            throws ServletException, IOException {
        response.setContentType("text/html;charset=UTF-8");
        PrintWriter out = response.getWriter();
        try {
             String uname=request.getParameter("username");
           String pass=request.getParameter("password");
           
           if(uname.equalsIgnoreCase("admin")&&pass.equalsIgnoreCase("admin")){
               HttpSession session=request.getSession(); 
               session.setAttribute("username",uname);
               response.sendRedirect("chome.jsp?status='registered'");
           }
           else{
               out.println("incorrect username or password ");
           }
        } 
        catch(Exception e){
            out.println(e);
        } finally {            
            out.close();
        }
    }

    // <editor-fold defaultstate="collapsed" desc="HttpServlet methods. Click on the + sign on the left to edit the code.">
    /**
     * Handles the HTTP
     * <code>GET</code> method.
     *
     * @param request servlet request
     * @param response servlet response
     * @throws ServletException if a servlet-specific error occurs
     * @throws IOException if an I/O error occurs
     */
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response)
            throws ServletException, IOException {
        processRequest(request, response);
    }

    /**
     * Handles the HTTP
     * <code>POST</code> method.
     *
     * @param request servlet request
     * @param response servlet response
     * @throws ServletException if a servlet-specific error occurs
     * @throws IOException if an I/O error occurs
     */
    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response)
            throws ServletException, IOException {
        processRequest(request, response);
    }

    /**
     * Returns a short description of the servlet.
     *
     * @return a String containing servlet description
     */
    @Override
    public String getServletInfo() {
        return "Short description";
    }// </editor-fold>
}

2.	User Login:

package action;

import java.io.IOException;
import java.io.PrintWriter;
import java.sql.Connection;
import java.sql.ResultSet;
import java.sql.Statement;
import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;
import pack.Dbconnection;

/**
 *
 * @author IBN5
 */
public class user_login extends HttpServlet {

    /**
     * Processes requests for both HTTP
     * <code>GET</code> and
     * <code>POST</code> methods.
     *
     * @param request servlet request
     * @param response servlet response
     * @throws ServletException if a servlet-specific error occurs
     * @throws IOException if an I/O error occurs
     */
    protected void processRequest(HttpServletRequest request, HttpServletResponse response)
            throws ServletException, IOException {
        response.setContentType("text/html;charset=UTF-8");
        PrintWriter out = response.getWriter();
        try {
            String uname=request.getParameter("username");
            String pass=request.getParameter("password");
            
          Connection con= Dbconnection.getConn();
          Statement st=con.createStatement();
          ResultSet rt=st.executeQuery("select * from user where emailid='"+uname+"' and password='"+pass+"' ");
          while(rt.next()){
              String p=rt.getString("password");
             
              String name=rt.getString("username");
              
                  if(rt.getString("role").equals("Doctor")){
                      HttpSession user=request.getSession(true);
                      user.setAttribute("name", name);
                      user.setAttribute("username", uname);
                      response.sendRedirect("ohome.jsp?status='registered'");
                  
                  }else
                  { 
                      HttpSession user=request.getSession(true);
                      user.setAttribute("name", name);
                      user.setAttribute("username", uname);
                      response.sendRedirect("uhome.jsp?status='registered'");
                      
          
                  }}
        
                
        
        
        
        }
        
        catch(Exception e){
            out.println(e);
        } finally {            
            out.close();
        }
    }

    // <editor-fold defaultstate="collapsed" desc="HttpServlet methods. Click on the + sign on the left to edit the code.">
    /**
     * Handles the HTTP
     * <code>GET</code> method.
     *
     * @param request servlet request
     * @param response servlet response
     * @throws ServletException if a servlet-specific error occurs
     * @throws IOException if an I/O error occurs
     */
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response)
            throws ServletException, IOException {
        processRequest(request, response);
    }

    /**
     * Handles the HTTP
     * <code>POST</code> method.
     *
     * @param request servlet request
     * @param response servlet response
     * @throws ServletException if a servlet-specific error occurs
     * @throws IOException if an I/O error occurs
     */
    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response)
            throws ServletException, IOException {
        processRequest(request, response);
    }

    /**
     * Returns a short description of the servlet.
     *
     * @return a String containing servlet description
     */
    @Override
    public String getServletInfo() {
        return "Short description";
    }// </editor-fold>
}

3.	Database Connection:

package pack;

import java.sql.Connection;
import java.sql.DriverManager;

/**
 *
 * @author JP Infotech
 */
public class Dbconnection {
    
    public static Connection getConn()throws Exception{
         Class.forName("com.mysql.jdbc.Driver");
         Connection con=DriverManager.getConnection("jdbc:mysql://localhost:3306/fitrecord","root","root");
        return con;
    
    }
   
}

4.	View Patient Diet: 

<html>
<head>
  <title>Food Recommended System</title>
  <meta name="description" content="website description" />
  <meta name="keywords" content="website keywords, website keywords" />
  <meta http-equiv="content-type" content="text/html; charset=UTF-8" />
  <link rel="shortcut icon" type="image/x-icon" href="images/brainstorming_alternative.png"/>
  <link rel="stylesheet" type="text/css" href="css/style.css" />
  <!-- modernizr enables HTML5 elements and feature detects -->
  <script type="text/javascript" src="js/modernizr-1.5.min.js"></script>
</head>

<body>
      <%
if(request.getParameter("no")!=null){
    out.println("<script>alert('Password Changed Successfully')</script>");
}    
if(request.getParameter("status")!=null){
    out.println("<script>alert('Login Successfully')</script>");
}

%>
  <div id="main">
    <header>
      <div id="logo">
        <div id="logo_text">
          <!-- class="logo_colour", allows you to change the colour of the text -->
        <h1 align="center"><img src="images/cooltext382455811891546.png"></h1></pre>
         
        </div>
      </div>
       <nav>
       
          <ul class="sf-menu" id="nav" style="margin-left:30px;">
              <li class="selected"><a href="ohome.jsp" style="background-color: darkviolet"><FONT COLOR="WHITE">Home</a></li>
              <li><a href="opassword.jsp" style="background-color: darkviolet"><FONT COLOR="WHITE">Change Password</a></li>
          <li><a href="#" style="background-color:darkviolet;color: white">Food Items</a>
          <ul>
                <li><a href="AddFoodItems.jsp">Add Food Items</a></li>
                <li><a href="ViewFoodItems.jsp">View Food Items</a></li>
             
            </ul>
          <li><a href="#" style="background-color: darkviolet;color: white">Patient Diet</a>
            <ul>
                <li><a href="AddPatientDiet.jsp">Add Diet's Schedule</a></li>
                <li><a href="ViewPatientDiet.jsp">View Patient Diets</a></li>
             
            </ul>
             
              
              <li><a href="cloud.jsp" style="background-color: darkviolet;color: white">Patient Requests</a>
              <ul>
                <li><a href="calculateBMI.jsp">BMI Calculation</a></li>
                <li><a href="calculateBMI.jsp">Diet Allocation</a></li>
              
             
            </ul>
              
               <li><a href="index.html" style="background-color: darkviolet;color: white">Logout</a></li>
              
          </li>
          
        </ul>
          
      </nav>
    </header>
    <div id="site_content">
       <%
HttpSession user=request.getSession();
String uname=user.getAttribute("username").toString();  
String name=user.getAttribute("name").toString();      
%>
        <div >
<div style="position: relative;left: 200px;background-color: darkturquoise">
   
    <h1 align="center"><font color="brown"><b>View Patient's Diet Schedule</b></font></h1>
    <form action="ViewPatientDiet2.jsp" method="post" style="margin-left: 20px">
        <% session.setAttribute("m1",request.getParameter("tk"));%>
        <label style="font-size: 20px">Disease Name</label>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<input type="text" name="t3" value="<%=request.getParameter("tk")%>" style="width: 200px;height: 20px;font-size: 20px;margin-left: 18px"/><br /><br />
         <form action="AddPatientDiet2.jsp" method="post" style="margin-left: 20px">
                    <label style="font-size: 20px">&nbsp;&nbsp;Diet Type</label>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
                    <select name="tk" style="width: 200px;height: 30px;font-size: 20px;margin-left:23px;" >
                        <option selected="">Select</option>
                        <option value="NormalDiet">Normal Diet</option>
                        <option value="Semi-solidDiet">Semi-solid Diet</option>
                        <option value="Full-LiquidDiet">Full-Liquid Diet</option>
                        
                      
                    </select>  <input type="Submit" value="SELECT" class="button" ></form><br />            
    
</div>
      </div>
    </div>
    <footer>
      <p>Copyright &copy; 2021. All Rights Reserved.</p>
    </footer>
  </div>
  <p>&nbsp;</p>
  <!-- javascript at the bottom for fast page loading -->
  <script type="text/javascript" src="js/jquery.js"></script>
  <script type="text/javascript" src="js/jquery.easing-sooper.js"></script>
  <script type="text/javascript" src="js/jquery.sooperfish.js"></script>
  <script type="text/javascript" src="js/image_fade.js"></script>
  <script type="text/javascript">
    $(document).ready(function() {
      $('ul.sf-menu').sooperfish();
    });
  </script>
</body>
</html>



5.	Calculate BMI:

<%@page import="java.sql.ResultSet"%>
<%@page import="java.sql.Statement"%>
<%@page import="pack.Dbconnection"%>
<%@page import="java.sql.Connection"%>
<!DOCTYPE HTML>
<html>

<head>
  <title>Food Recommended System</title>
  <meta name="description" content="website description" />
  <meta name="keywords" content="website keywords, website keywords" />
  <meta http-equiv="content-type" content="text/html; charset=UTF-8" />
  <link rel="shortcut icon" type="image/x-icon" href="images/brainstorming_alternative.png"/>
  <link rel="stylesheet" type="text/css" href="css/style.css" />
  <!-- modernizr enables HTML5 elements and feature detects -->
  <script type="text/javascript" src="js/modernizr-1.5.min.js"></script>
</head>

<body>
      <%
if(request.getParameter("allocate")!=null){
    out.println("<script>alert('The Diet is Allocated to Patient Successfully.!')</script>");
}    
if(request.getParameter("status")!=null){
    out.println("<script>alert('BMI Value is Calculated.!')</script>");
}

if(request.getParameter("no")!=null){
    out.println("<script>alert('Failure.!')</script>");
}
%>
  <div id="main">
    <header>
      <div id="logo">
        <div id="logo_text">
          <!-- class="logo_colour", allows you to change the colour of the text -->
        <h1 align="center"><img src="images/cooltext382455811891546.png"></h1></pre>
         
        </div>
      </div>
      <nav>
       
          <ul class="sf-menu" id="nav" style="margin-left:30px;">
              <li class="selected"><a href="ohome.jsp" style="background-color: darkviolet"><FONT COLOR="WHITE">Home</a></li>
              <li><a href="opassword.jsp" style="background-color: darkviolet"><FONT COLOR="WHITE">Change Password</a></li>
          <li><a href="#" style="background-color:darkviolet;color: white">Food Items</a>
          <ul>
                <li><a href="AddFoodItems.jsp">Add Food Items</a></li>
                <li><a href="ViewFoodItems.jsp">View Food Items</a></li>
             
            </ul>
          <li><a href="#" style="background-color: darkviolet;color: white">Patient Diet</a>
            <ul>
                <li><a href="AddPatientDiet.jsp">Add Diet's Schedule</a></li>
                <li><a href="ViewPatientDiet.jsp">View Patient Diets</a></li>
             
            </ul>
             
              
              <li><a href="cloud.jsp" style="background-color: darkviolet;color: white">Patient Requests</a>
              <ul>
                <li><a href="calculateBMI.jsp">BMI Calculation</a></li>
                <li><a href="calculateBMI.jsp">Diet Allocation</a></li>
              
             
            </ul>
              
               <li><a href="index.html" style="background-color: darkviolet;color: white">Logout</a></li>
              
          </li>
          
        </ul>
                  
      </nav>
    </header>
    <div id="site_content">
       <%
HttpSession user=request.getSession();
String uname=user.getAttribute("username").toString();  
String name=user.getAttribute("name").toString();      
%>
        <div >
<div style="position: relative;left: 100px;">
    
    <BR>
     <h4 >  <font color="brown"> Patient Disease Details</font></h4>
            
            <table bgcolor="white" cellpadding="5"  cellspacing="5" width="800" border="0" >
            <tr  bgcolor="#3300dd" align="center"> 
           <tr bgcolor="#ff0000" align="center"> 
              <td align="center" align="center"><font color="#110022"><strong>Patient Name</strong></font></td>   
              <td align="center"> <font color="#110022"><strong>Disease</strong></font></td>
              <td align="center"><font color="#110022"><strong>Pulses</strong></font></td>
              <td align="center"><font color="#110022"><strong>ECG</strong></font></td>
               <td align="center"><font color="#110022"><strong>Symptoms</strong></font></td>
               <td align="center"><font color="#110022"><strong>Height</strong></font></td>
                <td align="center"><font color="#110022"><strong>Weight</strong></font></td>
              <td align="center"><font color="#110022"><strong>Age</strong></font></td>
               <td align="center"><font color="#110022"><strong>BMI Value</strong></font></td>
               <td align="center"><font color="#110022"><strong>Diet Status</strong></font></td>
              
            </tr>
            
<%               
    
    //name, userid, pass, mail, age, loc, sex, time_
        String  u=null,st=null,en=null,intr=null,dot=null,dy=null,nop=null,sta=null,toc=null,key=null;
        

Connection con= Dbconnection.getConn();
Statement st1 = con.createStatement();

ResultSet rs1 = st1.executeQuery("select * from patientdetails");
while(rs1.next()){

       %>   
      <tr bgcolor="#e0e0e0" align="center"> 
<td align="center"> <strong><font color="blue"> <%=rs1.getString(2)%> </font></strong></td>
              
          
             <td align="center"><strong><font  color="blue"><%=rs1.getString(4)%></font></a></strong></td>
            
<td align="center"><strong><font  color="blue"><%=rs1.getString(5)%></font></a></strong></td>
 <td align="center"><strong><font  color="blue"><%=rs1.getString(6)%></font></a></strong></td>
            

       <td align="center"><strong><font  color="blue"><%=rs1.getString(7)%></font></a></strong></td>
        <td align="center"><strong><font  color="purple"><%=rs1.getString(8)%></font></a></strong></td>
         <td align="center"><strong><font  color="purple"><%=rs1.getString(9)%></font></a></strong></td>
        <td align="center"><strong><font  color="purple"><%=rs1.getString(10)%></font></a></strong></td>
       
               <%if(rs1.getString("bmi").equals("Waiting")){%>
       
               <td align="center"><strong><a href="calculateBMI1.jsp?<%=rs1.getString(1)%>">Calculate</font></a></strong></td>
               <%}else{%> 
               
              <td align="center"><strong><font  color="purple"><%=rs1.getString("bmi")%></font></a></strong></td>
       
          
          <%}%>
          
          
          
          
          <%if(rs1.getString("dietstatus").equals("Waiting")){%>
            
        <td align="center"><strong><a href="addDiettoPatient.jsp?<%=rs1.getString(1)%>"><img src="images/cooltext382427693958874.png" width="150" height="50"></font></a></strong></td>
        <%}else{%> 
               
        <td align="center"><strong><a href="ViewPatientdietDetails.jsp?pname=<%=rs1.getString(2)%>&&disease=<%=rs1.getString(4)%>"><img src="images/brainstorming_alternative.png" height="50" width="50"></a> </font></a></strong></td>
       
          
          <%}%>
        
           </form>
     <%

}%>
</tr>
                                          </table><BR>
    
</div>
      </div>
    </div>
    <footer>
      <p>Copyright &copy; 2021. All Rights Reserved.</p>
    </footer>
  </div>
  <p>&nbsp;</p>
  <!-- javascript at the bottom for fast page loading -->
  <script type="text/javascript" src="js/jquery.js"></script>
  <script type="text/javascript" src="js/jquery.easing-sooper.js"></script>
  <script type="text/javascript" src="js/jquery.sooperfish.js"></script>
  <script type="text/javascript" src="js/image_fade.js"></script>
  <script type="text/javascript">
    $(document).ready(function() {
      $('ul.sf-menu').sooperfish();
    });
  </script>
</body>
</html>

  
  Conclusion : 


Metabolic disorders could be addressed with “Diet Suggestion” software described by this report. The software suggests diets based on the current metabolic needs of a patients by means of accurate BMI calculation and correlation of BMI and nutritional values in a diet. Also, for calculating BMI, a good amount of data was collected in the case study.
What disorder requires what kind of food? Let’s say to fight cardiovascular disease, a patient requires a high protein diet plan. The intake of fats, carbohydrates and proteins is stored in the database.
      The food items and calories stored for different food items are best described in Figure
 
 Food Items and Calories stored for each food item.

A web-application is only limited by the speed of the internet and the ability of a device to browse the internet. The web application doesn’t require any expensive hardware on the end-user side and doesn’t require to be coded specifically for different OS, giving the end-user a choice to use any device, thus making the process less expensive than a desktop or even a mobile application.



