# 🧾 MemberDto.java

```java
package Utils;

public class MemberDto {
	private String m_no;
	private String m_name;
	private String p_name;
	private String p_school;
	private String m_jumin;
	private String m_city;
	private String p_tel1;
	private String p_tel2;
	private String p_tel3;

	public MemberDto() {}

	public MemberDto(String m_no, String m_name, String p_name, String p_school, String m_jumin, String m_city,
			String p_tel1, String p_tel2, String p_tel3) {
		super();
		this.m_no = m_no;
		this.m_name = m_name;
		this.p_name = p_name;
		this.p_school = p_school;
		this.m_jumin = m_jumin;
		this.m_city = m_city;
		this.p_tel1 = p_tel1;
		this.p_tel2 = p_tel2;
		this.p_tel3 = p_tel3;
	}

	public String getM_no() { return m_no; }
	public void setM_no(String m_no) { this.m_no = m_no; }
	public String getM_name() { return m_name; }
	public void setM_name(String m_name) { this.m_name = m_name; }
	public String getP_name() { return p_name; }
	public void setP_name(String p_name) { this.p_name = p_name; }
	public String getP_school() { return p_school; }
	public void setP_school(String p_school) { this.p_school = p_school; }
	public String getM_jumin() { return m_jumin; }
	public void setM_jumin(String m_jumin) { this.m_jumin = m_jumin; }
	public String getM_city() { return m_city; }
	public void setM_city(String m_city) { this.m_city = m_city; }
	public String getP_tel1() { return p_tel1; }
	public void setP_tel1(String p_tel1) { this.p_tel1 = p_tel1; }
	public String getP_tel2() { return p_tel2; }
	public void setP_tel2(String p_tel2) { this.p_tel2 = p_tel2; }
	public String getP_tel3() { return p_tel3; }
	public void setP_tel3(String p_tel3) { this.p_tel3 = p_tel3; }

	@Override
	public String toString() {
		return "MemberDto [m_no=" + m_no + ", m_name=" + m_name + ", p_name=" + p_name + ", p_school=" + p_school
				+ ", m_jumin=" + m_jumin + ", m_city=" + m_city + ", p_tel1=" + p_tel1 + ", p_tel2=" + p_tel2
				+ ", p_tel3=" + p_tel3 + "]";
	}
}
```

# 🗳️ VoteDto.java

```java
package Utils;

public class VoteDto {
	private String v_jumin;
	private String v_name;
	private String m_no;
	private String v_time;
	private String v_area;
	private String v_confirm;

	public String getV_jumin() { return v_jumin; }
	public void setV_jumin(String v_jumin) { this.v_jumin = v_jumin; }
	public String getV_name() { return v_name; }
	public void setV_name(String v_name) { this.v_name = v_name; }
	public String getM_no() { return m_no; }
	public void setM_no(String m_no) { this.m_no = m_no; }
	public String getV_time() { return v_time; }
	public void setV_time(String v_time) { this.v_time = v_time; }
	public String getV_area() { return v_area; }
	public void setV_area(String v_area) { this.v_area = v_area; }
	public String getV_confirm() { return v_confirm; }
	public void setV_confirm(String v_confirm) { this.v_confirm = v_confirm; }

	@Override
	public String toString() {
		return "VoteDto [v_jumin=" + v_jumin + ", v_name=" + v_name + ", m_no=" + m_no + ", v_time=" + v_time
				+ ", v_area=" + v_area + ", v_confirm=" + v_confirm + "]";
	}

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
}
```

# 🛠️ DBUtils.java

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

	public List<MemberDto> selectAllMember() throws Exception {
		String sql="select M.M_NO,M.M_NAME,P.P_NAME,M.P_SCHOOL,M.M_JUMIN,M.M_CITY,P.P_TEL1,P.P_TEL2,P.P_TEL3"
				+ " from TBL_MEMBER_202005 M"
				+ " join TBL_PARTY_202005 P"
				+ " on M.P_CODE=P.P_CODE";
		pstmt = conn.prepareStatement(sql);
		rs = pstmt.executeQuery();
		List<MemberDto> list = new ArrayList();
		MemberDto dto = null;
		if(rs!=null) {
			while(rs.next()) {
				dto = new MemberDto();	
				dto.setM_no(rs.getString(1));
				dto.setM_name(rs.getString(2));
				dto.setP_name(rs.getString(3));
				dto.setP_school(rs.getString(4));
				dto.setM_jumin(rs.getString(5));
				dto.setM_city(rs.getString(6));
				dto.setP_tel1(rs.getString(7));
				dto.setP_tel2(rs.getString(8));
				dto.setP_tel3(rs.getString(9));
				list.add(dto);
			}
		}
		pstmt.close();
		rs.close();
		return list;
	}

	public int insertVote(VoteDto dto) throws Exception {
		pstmt = conn.prepareStatement("insert into TBL_VOTE_202005 values(?,?,?,?,?,?)");
		pstmt.setString(1, dto.getV_jumin());
		pstmt.setString(2, dto.getV_name());
		pstmt.setString(3, dto.getM_no());
		pstmt.setString(4, dto.getV_time());
		pstmt.setString(5, dto.getV_area());
		pstmt.setString(6, dto.getV_confirm());
		int result = pstmt.executeUpdate();
		pstmt.close();
		return result;
	}

	public List<VoteDto> selectAllVote() throws Exception {
		String sql="select * from TBL_VOTE_202005";
		pstmt = conn.prepareStatement(sql);
		rs = pstmt.executeQuery();
		List<VoteDto> list = new ArrayList();
		VoteDto dto = null;
		if(rs!=null) {
			while(rs.next()) {
				dto = new VoteDto();
				dto.setV_jumin(rs.getString(1));
				dto.setV_name(rs.getString(2));
				dto.setM_no(rs.getString(3));
				dto.setV_time(rs.getString(4));
				dto.setV_area(rs.getString(5));
				dto.setV_confirm(rs.getString(6));
				list.add(dto);
			}
		}
		pstmt.close();
		rs.close();
		return list;
	}
}
```