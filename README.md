# codepackage DB;

import java.sql.Statement;

import javax.swing.JOptionPane;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.ResultSet;

public class Member_DB {
	private Connection con;
	private Statement st;
	private ResultSet rs;
	private PreparedStatement pstmt;
	
	public Member_DB() {
		
	}

	public void Connect() {
		try {
			Class.forName("com.mysql.jdbc.Driver");
			con = DriverManager.getConnection("jdbc:mysql://127.0.0.1/MEMBER_DB?" + "serverTimezone=UTC&useSSL=false",
					"root", "");
			st = con.createStatement();

		} catch (Exception e) {
			System.out.println(e.getMessage());
		}
	}

	public boolean Try_Login(String input_id, String input_password) {
		String SQL = "SELECT * FROM MEMBER";
		try {
			rs = st.executeQuery(SQL);
			while (rs.next()) {
				String id = rs.getString("id");
				String password = rs.getString("password");


				if (input_id.contentEquals(id) == true) {
					if (input_password.contentEquals(password) == true) {
						JOptionPane.showMessageDialog(null, "아이디, 패스워드 일치.");
						return true;
					} else {
						JOptionPane.showMessageDialog(null, "패스워드 불일치.");
						return false;
					}
				} else {
					JOptionPane.showMessageDialog(null, "없는 유저입니다.");
					return false;
				}
			}
		} catch (Exception e) {
			System.out.println(e.getMessage());
		}
		return false;
	}
	public boolean Try_Sign(String input_id, String input_email) {
		String SQL = "SELECT * FROM MEMBER";
		try {
			rs = st.executeQuery(SQL);
			while (rs.next()) {
				String id = rs.getString("id");
				String email = rs.getString("email");
				
				if(input_id.contentEquals(id) == true) {
					JOptionPane.showMessageDialog(null, "이미 존재하는 아이디입니다.");
					return false;
				}else if(input_email.contentEquals(email) == true) {
					JOptionPane.showMessageDialog(null, "사용중인 이메일입니다.");
					return false;
				}else {
					JOptionPane.showMessageDialog(null, "사용할 수 있는 정보입니다.");
					return true;
				}
			}
		} catch (Exception e) {
			System.out.println(e.getMessage());
		}
		return false;
	}
	public void add_data(String input_id, String input_password, String input_email) {
		PreparedStatement pstmt = null;
		String SQL = "insert into member values(?, ?, ?)";
		try {
			pstmt=con.prepareStatement(SQL);
			pstmt.setString(1, input_id);
			pstmt.setString(2, input_password);
			pstmt.setString(3, input_password);
			pstmt.executeUpdate();
		} catch (Exception e) {
			// TODO: handle exception
		}
	}
}
