
---

# ClassDto.java

```java
package Utils;

public class ClassDto {
	private String regist_month;
	private String c_no;
	private String class_area;
	private String tuition;
	private String teacher_code;

	public ClassDto() {}

	public ClassDto(String regist_month, String c_no, String class_area, String tuition, String teacher_code) {
		super();
		this.regist_month = regist_month;
		this.c_no = c_no;
		this.class_area = class_area;
		this.tuition = tuition;
		this.teacher_code = teacher_code;
	}

	public String getRegist_month() {
		return regist_month;
	}
	public void setRegist_month(String regist_month) {
		this.regist_month = regist_month;
	}
	public String getC_no() {
		return c_no;
	}
	public void setC_no(String c_no) {
		this.c_no = c_no;
	}
	public String getClass_area() {
		return class_area;
	}
	public void setClass_area(String class_area) {
		this.class_area = class_area;
	}
	public String getTuition() {
		return tuition;
	}
	public void setTuition(String tuition) {
		this.tuition = tuition;
	}
	public String getTeacher_code() {
		return teacher_code;
	}
	public void setTeacher_code(String teacher_code) {
		this.teacher_code = teacher_code;
	}

	@Override
	public String toString() {
		return "ClassDto [regist_month=" + regist_month + ", c_no=" + c_no + ", class_area=" + class_area + ", tuition="
				+ tuition + ", teacher_code=" + teacher_code + "]";
	}
}
```

---

# DBUtils.java

```java
package Utils;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.util.ArrayList;
import java.util.List;

public class DBUtils {
	
	private String url = "jdbc:oracle:thin:@localhost:1521:xe";
	private String id = "system";
	private String pw = "1234";
	
	private Connection conn;
	private PreparedStatement pstmt;
	private ResultSet rs;	
	
	private static DBUtils instance;
	private DBUtils() throws Exception {
		Class.forName("oracle.jdbc.driver.OracleDriver");
		conn = DriverManager.getConnection(url, id, pw);
	}
	public static DBUtils getInstance() throws Exception {
		if(instance==null)
			instance = new DBUtils();
		return instance;
	}
	
	public List<TeacherDto> selectAllTeacher() throws Exception {
		String sql="select * from TBL_TEACHER_202201";
		pstmt = conn.prepareStatement(sql);
		rs = pstmt.executeQuery();
		List<TeacherDto> list = new ArrayList();
		TeacherDto dto = null;
		if(rs!=null) {
			while(rs.next()) {
				dto=new TeacherDto();
				dto.setTeacher_code(rs.getString(1));
				dto.setTeacher_name(rs.getString(2));
				dto.setClass_name(rs.getString(3));
				dto.setClass_price(rs.getInt(4));
				dto.setTeacher_regist_date(rs.getString(5));
				list.add(dto);
			}
		}
		return list;
	}

	public List<MemberDto> selectAllMember() throws Exception{
		String sql="select * from TBL_MEMBER_202201";
		pstmt = conn.prepareStatement(sql);
		rs = pstmt.executeQuery();
		List<MemberDto> list = new ArrayList();
		MemberDto dto = null;
		if(rs!=null) {
			while(rs.next()) {
				dto=new MemberDto();
				dto.setC_no(rs.getString(1));
				dto.setC_name(rs.getString(2));
				dto.setPhone(rs.getString(3));
				dto.setAddress(rs.getString(4));
				dto.setGrade(rs.getString(5));
				list.add(dto);
			}
		}
		rs.close();
		pstmt.close();
		return list;
	}
	
	public List<ClassDto> selectAllClass() throws Exception{
		String sql="select * from TBL_CLASS_202201";
		pstmt = conn.prepareStatement(sql);
		rs = pstmt.executeQuery();
		List<ClassDto> list = new ArrayList();
		ClassDto dto = null;
		if(rs!=null) {
			while(rs.next()) {
				dto=new ClassDto();
				dto.setRegist_month(rs.getString(1));
				dto.setC_no(rs.getString(2));
				dto.setClass_area(rs.getString(3));
				dto.setTuition(rs.getString(4));
				dto.setTeacher_code(rs.getString(5));
				list.add(dto);
			}
		}
		rs.close();
		pstmt.close();
		return list;
	}
	
	public int insertClass(ClassDto classDto) throws Exception{
		pstmt = conn.prepareStatement("insert into TBL_CLASS_202201 values(?,?,?,?,?)");
		pstmt.setString(1, classDto.getRegist_month());
		pstmt.setString(2, classDto.getC_no());
		pstmt.setString(3, classDto.getClass_area());
		pstmt.setString(4, classDto.getTuition());
		pstmt.setString(5, classDto.getTeacher_code());
		int result = pstmt.executeUpdate();
		conn.commit();
		pstmt.close();
		return result;
	}
	
	public List<Join1Dto> selectAllJoin1() throws Exception{
		String sql="SELECT C.REGIST_MONTH,M.C_NO,M.C_NAME,T.CLASS_NAME,C.CLASS_AREA,C.TUITION,M.GRADE"
				+ " FROM TBL_MEMBER_202201 M"
				+ " JOIN TBL_CLASS_202201 C"
				+ " ON C.C_NO=M.C_NO"
				+ " JOIN TBL_TEACHER_202201 T"
				+ " ON C.TEACHER_CODE=T.TEACHER_CODE";
		pstmt = conn.prepareStatement(sql);
		rs = pstmt.executeQuery();
		List<Join1Dto> list = new ArrayList();
		Join1Dto dto = null;
		if(rs!=null) {
			while(rs.next()) {
				dto=new Join1Dto();
				dto.setRegist_month(rs.getString(1));
				dto.setC_no(rs.getString(2));
				dto.setC_name(rs.getString(3));
				dto.setClass_name(rs.getString(4));
				dto.setClass_area(rs.getString(5));
				dto.setTuition(rs.getString(6));
				dto.setGrade(rs.getString(7));
				list.add(dto);
			}
		}
		rs.close();
		pstmt.close();
		return list;
	}
	
	public List<Join2Dto> selectAllJoin2() throws Exception{
		String sql="SELECT T.TEACHER_CODE,T.CLASS_NAME,T.TEACHER_NAME,SUM(C.TUITION)"
				+ " FROM TBL_CLASS_202201 C"
				+ " JOIN TBL_TEACHER_202201 T"
				+ " ON C.TEACHER_CODE = T.TEACHER_CODE"
				+ " GROUP BY T.TEACHER_CODE,T.CLASS_NAME,T.TEACHER_NAME"
				+ " ORDER BY SUM(C.TUITION) desc";
		pstmt = conn.prepareStatement(sql);
		rs = pstmt.executeQuery();
		List<Join2Dto> list = new ArrayList();
		Join2Dto dto = null;
		if(rs!=null) {
			while(rs.next()) {
				dto=new Join2Dto();
				dto.setTeacher_code(rs.getString(1));
				dto.setClass_name(rs.getString(2));
				dto.setTeacher_name(rs.getString(3));
				dto.setTotal_tuition(rs.getString(4));
				list.add(dto);
			}
		}
		rs.close();
		pstmt.close();
		return list;
	}
}
```

---

# Join1Dto.java

```java
package Utils;

public class Join1Dto {
	private String regist_month;
	private String c_no;
	private String c_name;
	private String class_name;
	private String class_area;
	private String tuition;
	private String grade;

	public Join1Dto(){}

	public Join1Dto(String regist_month, String c_no, String c_name, String class_name, String class_area,
			String tuition, String grade) {
		super();
		this.regist_month = regist_month;
		this.c_no = c_no;
		this.c_name = c_name;
		this.class_name = class_name;
		this.class_area = class_area;
		this.tuition = tuition;
		this.grade = grade;
	}

	public String getRegist_month() {
		return regist_month;
	}
	public void setRegist_month(String regist_month) {
		this.regist_month = regist_month;
	}
	public String getC_no() {
		return c_no;
	}
	public void setC_no(String c_no) {
		this.c_no = c_no;
	}
	public String getC_name() {
		return c_name;
	}
	public void setC_name(String c_name) {
		this.c_name = c_name;
	}
	public String getClass_name() {
		return class_name;
	}
	public void setClass_name(String class_name) {
		this.class_name = class_name;
	}
	public String getClass_area() {
		return class_area;
	}
	public void setClass_area(String class_area) {
		this.class_area = class_area;
	}
	public String getTuition() {
		return tuition;
	}
	public void setTuition(String tuition) {
		this.tuition = tuition;
	}
	public String getGrade() {
		return grade;
	}
	public void setGrade(String grade) {
		this.grade = grade;
	}

	@Override
	public String toString() {
		return "Join1Dto [regist_month=" + regist_month + ", c_no=" + c_no + ", c_name=" + c_name + ", class_name="
				+ class_name + ", class_area=" + class_area + ", tuition=" + tuition + ", grade=" + grade + "]";
	}
}
```

---

# Join2Dto.java

```java
package Utils;

public class Join2Dto {
	private String teacher_code;
	private String class_name;
	private String teacher_name;
	private String total_tuition;

	public Join2Dto() {}

	public Join2Dto(String teacher_code, String class_name, String teacher_name, String total_tuition) {
		super();
		this.teacher_code = teacher_code;
		this.class_name = class_name;
		this.teacher_name = teacher_name;
		this.total_tuition = total_tuition;
	}

	public String getTeacher_code() {
		return teacher_code;
	}
	public void setTeacher_code(String teacher_code) {
		this.teacher_code = teacher_code;
	}
	public String getClass_name() {
		return class_name;
	}
	public void setClass_name(String class_name) {
		this.class_name = class_name;
	}
	public String getTeacher_name() {
		return teacher_name;
	}
	public void setTeacher_name(String teacher_name) {
		this.teacher_name = teacher_name;
	}
	public String getTotal_tuition() {
		return total_tuition;
	}
	public void setTotal_tuition(String total_tuition) {
		this.total_tuition = total_tuition;
	}

	@Override
	public String toString() {
		return "Join2Dto [teacher_code=" + teacher_code + ", class_name=" + class_name + ", teacher_name="
				+ teacher_name + ", total_tuition=" + total_tuition + "]";
	}
}
```

---

# MemberDto.java

```java
package Utils;

public class MemberDto {
	private String c_no;
	private String c_name;
	private String phone;
	private String address;
	private String grade;

	public MemberDto() {}

	public MemberDto(String c_no, String c_name, String phone, String address, String grade) {
		super();
		this.c_no = c_no;
		this.c_name = c_name;
		this.phone = phone;
		this.address = address;
		this.grade = grade;
	}

	public String getC_no() {
		return c_no;
	}
	public void setC_no(String c_no) {
		this.c_no = c_no;
	}
	public String getC_name() {
		return c_name;
	}
	public void setC_name(String c_name) {
		this.c_name = c_name;
	}
	public String getPhone() {
		return phone;
	}
	public void setPhone(String phone) {
		this.phone = phone;
	}
	public String getAddress() {
		return address;
	}
	public void setAddress(String address) {
		this.address = address;
	}
	public String getGrade() {
		return grade;
	}
	public void setGrade(String grade) {
		this.grade = grade;
	}

	@Override
	public String toString() {
		return "MemberDto [c_no=" + c_no + ", c_name=" + c_name + ", phone=" + phone + ", address=" + address
				+ ", grade=" + grade + "]";
	}
}
```

---

# TeacherDto.java

```java
package Utils;

public class TeacherDto {
	private String teacher_code;
	private String teacher_name;
	private String class_name;
	private int class_price;
	private String teacher_regist_date;

	public TeacherDto() {}

	public TeacherDto(String teacher_code, String teacher_name, String class_name, int class_price,
			String teacher_regist_date) {
		super();
		this.teacher_code = teacher_code;
		this.teacher_name = teacher_name;
		this.class_name = class_name;
		this.class_price = class_price;
		this.teacher_regist_date = teacher_regist_date;
	}

	public String getTeacher_code() {
		return teacher_code;
	}
	public void setTeacher_code(String teacher_code) {
		this.teacher_code = teacher_code;
	}
	public String getTeacher_name() {
		return teacher_name;
	}
	public void setTeacher_name(String teacher_name) {
		this.teacher_name = teacher_name;
	}
	public String getClass_name() {
		return class_name;
	}
	public void setClass_name(String class_name) {
		this.class_name = class_name;
	}
	public int getClass_price() {
		return class_price;
	}
	public void setClass_price(int class_price) {
		this.class_price = class_price;
	}
	public String getTeacher_regist_date() {
		return teacher_regist_date;
	}
	public void setTeacher_regist_date(String teacher_regist_date) {
		this.teacher_regist_date = teacher_regist_date;
	}

	@Override
	public String toString() {
		return "TeacherDto [teacher_code=" + teacher_code + ", teacher_name=" + teacher_name + ", class_name="
				+ class_name + ", class_price=" + class_price + ", teacher_regist_date=" + teacher_regist_date + "]";
	}
}
```

---

# VoteDto.java

```java
package Utils;

public class VoteDto {
	private String v_jumin;
	private String v_name;
	private String m_no;
	private String v_time;
	private String v_area;
	private String v_confirm;

	public VoteDto(String v_jumin, String v_name, String m_no, String v_time, String v_area, String v_confirm) {
		super();
		this.v_jumin = v_jumin;
		this.v_name = v_name;
		this.m_no = m_no;
		this.v_time = v_time;
		this.v_area = v_area;
		this.v_confirm = v_confirm;
	}

	public VoteDto() {}

	public String getV_jumin() {
		return v_jumin;
	}
	public void setV_jumin(String v_jumin) {
		this.v_jumin = v_jumin;
	}
	public String getV_name() {
		return v_name;
	}
	public void setV_name(String v_name) {
		this.v_name = v_name;
	}
	public String getM_no() {
		return m_no;
	}
	public void setM_no(String m_no) {
		this.m_no = m_no;
	}
	public String getV_time() {
		return v_time;
	}
	public void setV_time(String v_time) {
		this.v_time = v_time;
	}
	public String getV_area() {
		return v_area;
	}
	public void setV_area(String v_area) {
		this.v_area = v_area;
	}
	public String getV_confirm() {
		return v_confirm;
	}
	public void setV_confirm(String v_confirm) {
		this.v_confirm = v_confirm;
	}

	@Override
	public String toString() {
		return "VoteDto [v_jumin=" + v_jumin + ", v_name=" + v_name + ", m_no=" + m_no + ", v_time=" + v_time
				+ ", v_area=" + v_area + ", v_confirm=" + v_confirm + "]";
	}
}
```

---
