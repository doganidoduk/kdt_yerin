```
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.util.ArrayList;
import java.util.List;
import java.sql.Statement;

import boardapp.model.BoardDto;

public class BoardDao {
    private Connection conn;
    private PreparedStatement pstmt;
     private ResultSet rs;

    public BoardDao() throws Exception {
        String url = "jdbc:mysql://localhost:3306/board_db?useSSL=false&serverTimezone=UTC";
        String user = "root";
        String password = "1234"; // 🔁 여기에 실제 비밀번호 입력

        conn = DriverManager.getConnection(url, user, password);
    }

     public void insert(BoardDto board) throws Exception {
        String sql = "INSERT INTO tb_board (title, writer, content) VALUES (?, ?, ?)";
        try (PreparedStatement pstmt = conn.prepareStatement(sql)) {
            pstmt.setString(1, board.getTitle());
            pstmt.setString(2, board.getWriter());
            pstmt.setString(3, board.getContent());
            pstmt.executeUpdate();
        }
    }

    public List<BoardDto> selectAll() throws Exception {
        List<BoardDto> list = new ArrayList<>();
        String sql = "SELECT * FROM tb_board ORDER BY no DESC";

        try (Statement stmt = conn.createStatement();
             ResultSet rs = stmt.executeQuery(sql)) {

            while (rs.next()) {
                BoardDto b = new BoardDto();
                b.setNo(rs.getInt("no"));
                b.setTitle(rs.getString("title"));
                b.setWriter(rs.getString("writer"));
                b.setContent(rs.getString("content"));
                b.setDate(rs.getString("date"));
                b.setHitcount(rs.getInt("hitcount"));
                list.add(b);
            }
        }
        return list;
    }

    public int update(BoardDto dto) {
    int result = 0;
    try {
        String sql = "UPDATE tb_board SET title=?, content=? WHERE no=?";
        pstmt = conn.prepareStatement(sql);
        pstmt.setString(1, dto.getTitle());
        pstmt.setString(2, dto.getContent());
        pstmt.setInt(3, dto.getNo());

        result = pstmt.executeUpdate();
    } catch (Exception e) {
        e.printStackTrace();
    }
    return result;
}
     public void close() throws SQLException {
        if (conn != null) conn.close();
     }
    

     public int delete(int no) {
    int result = 0;
    try {
        String sql = "DELETE FROM tb_board WHERE no = ?";
        pstmt = conn.prepareStatement(sql);
        pstmt.setInt(1, no);
        result = pstmt.executeUpdate();
    } catch (Exception e) {
        e.printStackTrace();
    }
    return result;
}

// BoardDao.java 안에 추가
public BoardDto selectByNo(int no) throws Exception {
    BoardDto dto = null;
    String sql = "SELECT * FROM tb_board WHERE no = ?";

    pstmt = conn.prepareStatement(sql);
    pstmt.setInt(1, no);
    rs = pstmt.executeQuery();

    if (rs.next()) {
        dto = new BoardDto();
        dto.setNo(rs.getInt("no"));
        dto.setTitle(rs.getString("title"));
        dto.setWriter(rs.getString("writer"));
        dto.setContent(rs.getString("content"));
        dto.setDate(rs.getString("date"));
        dto.setHitcount(rs.getInt("hitcount"));
    }

    return dto;
}

}
```
