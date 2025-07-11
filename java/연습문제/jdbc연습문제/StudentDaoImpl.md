```
public class StudentDaoImpl implements StudentDAO{

    /** 데이터 베이스 접속 객체 */
    private Connection conn;

    /** 생성자를 통해서 데이터베이스 접속 객체를 전달 받는다.*/
    public StudentDaoImpl (Connection conn) {
        this.conn = conn;
    }

    @Override
    public int delete(int params) {
   int result = 0;

        /** 실행할 SQL 구문 정의 */
        String sql = "DELETE FROM department WHERE deptno=?";

        /** SQL 구문 실행하기 위한 객체 */
        // --> import java.sql.PreparedStatement;
        PreparedStatement pstmt = null;

        /** SQL 구문 처리하기 */
        try {
            // pstmt 객체 할당
            pstmt = conn.prepareStatement(sql);

            // 템플릿에 데이터 설정
            pstmt.setInt(1, params);

            // SQL문 실행하기 --> 결과 행의 수를 리턴할 변수에 대입함
              result = pstmt.executeUpdate();
        } catch (SQLException e) {
            System.out.println("MySQL SQL Fail : " + e.getMessage());
        } finally {
            if (pstmt != null) {
                // 객체 닫기
                try {
                    pstmt.close();
                } catch (SQLException e) {}
            }
        }

        return result;
    }

    @Override
    public int insert(Student params) {
      int result = 0;

        /** 실행할 SQL구문 정의*/
        String sql = "INSERT INTO department (dname,loc) VALUES(?,?)";

        /** SQL 구문 실행하기 위한 객체*/
        // -->import java.sql.preparedStatement;
        PreparedStatement pstmt = null;
        // -->import java.sql.ResultSet;
        ResultSet rs = null;

        /** SQL 구문 처리하기*/

        try {
            // pstmt 객체 할당
            pstmt = conn.prepareStatement(sql, Statement.RETURN_GENERATED_KEYS);

            // 템플릿에 데이터 설정
            pstmt.setInt(1, params.getStdno());
            pstmt.setString(2, params.getStdname());
            pstmt.setString(3, params.getPhone());
            pstmt.setString(4, params.getEmail());

            // SQL문 실행하기 --> 결과 행의 수를 리턴할 변수에 대입함
            pstmt.executeUpdate();

            // Primary Key 받기
            rs = pstmt.getGeneratedKeys();
            rs.next();
            result = rs.getInt(1);

        } catch (SQLException e) {
            System.out.println("MySQL SQL Fail : " + e.getMessage());
        } finally {
            if(rs != null) {
                // 객체 닫기
                try {
                    rs.close();
                } catch (Exception e) {}
            }
            if (pstmt != null) {
                // 객체 닫기
                try {
                    pstmt.close();
                } catch (SQLException e) {}
            }
        }

        return result;
    }

    @Override
    public List<Student> select() {
        List<Student> result = null;

        // 'department' 테이블에 데이터를 갱신하기 위한 SQL의 템플릿
        String sql = "SELECT * FROM student";

        /** SQL 구문 실행하기 위한 객체 */
        // --> import java.sql.PreparedStatement;
        PreparedStatement pstmt = null;
        // --> import java.sql.ResultSet;
        ResultSet rs = null;

        try {
            // pstmt 객체 할당
            pstmt = conn.prepareStatement(sql);

            // SELECT 구문을 실행한 후, 결과셋을 리턴받는다.
            rs = pstmt.executeQuery();

            /** SQL 결과를 컬렉션에 할당*/
            // SQL이 실행되므로 컬렉션을 할당한다.
            result = new ArrayList<Student>();

            // 한 줄씩 스캔하는 반복문 구성
            while (rs.next()) {
                int stdno = rs.getInt("stdno");
                String stdname = rs.getString("stdname");
                String phone = rs.getString("phone");
                String email = rs.getString("email");

                Student item = new Student(stdno,stdname,phone,email);
                result.add(item);
            }
        } catch (SQLException e) {
            System.out.println("MySQL SQL Fail : " + e.getMessage());
        } finally {
            // 객체를 생성한 순서의 역순으로 객체를 닫는다.
            if ( rs != null) {
                try {
                    rs.close();
                } catch (SQLException e) { }
            }
            if ( pstmt != null) {
                try {
                    pstmt.close();
                } catch (SQLException e) { }
            }
        }
        return result;
    }


    @Override
    public Student selectOne(int params) {
        Student result = null;

        /** 실행할 SQL 구문 정의 */
        String sql = "SELECT stdno, stdname, phone, email FROM department WHERE stdno=?";

        /** SQL 구문 실행하기 위한 객체 */
        // --> import java.sql.PreparedStatement
        PreparedStatement pstmt = null;
        // --> import java.sql.ResultSet;
        ResultSet rs = null;

        /** SQL 구문 처리하기 */
        try {
            pstmt = conn.prepareStatement(sql);

            // 템플릿에 데이터 설정
            pstmt.setInt(1, params);

            // SQL문 실행하기 --> 결과 행 리턴됨
            rs = pstmt.executeQuery();

            // 조회 결과의 첫 번째 줄로 이동
            boolean first = rs.next();

            if(first) {
                // SELECT절에 명시한 컬럼 이름을 사용하여 데이터 추출
                int stdno = rs.getInt("stdno");
                String stdname = rs.getString("stdname");
                String phone = rs.getString("phone");
                String email = rs.getString("email");

                // 리턴할 객체에 조회한 값을 사용하여 객체를 할당한다.
                result = new Student(stdno, stdname, phone, email);
            } else {
                System.out.println("조회 결과가 없습니다.");
            }
        } catch (SQLException e) {
            System.out.println("MySQL SQL Fail : " + e.getMessage());
        } finally {
            // 객체를 생성한 순서의 역순으로 객체를 닫는다.
            if ( rs != null) {
                try {
                    rs.close();
                } catch (SQLException e) { }
            }
            if ( pstmt != null) {
                try {
                    pstmt.close();
                } catch (SQLException e) { }
            }
        }
        return result;
    }

    @Override
    public int update(Student params) {
  int result = 0;

        /** 실행할 SQL 구문 정의 */
        String sql = "UPDATE department SET dname=?, loc=? WHERE deptno=?";

        /** SQL 구문 실행하기 위한 객체 */
        // --> import java.sql.PreparedStatement;
        PreparedStatement pstmt = null;

        /** SQL 구문 처리하기 */
        try {
            // pstmt 객체 할당
            pstmt = conn.prepareStatement(sql);

            // 템플릿에 데이터 설정
            pstmt.setInt(1, params.getStdno());
            pstmt.setString(2, params.getStdname());
            pstmt.setString(3, params.getPhone());
            pstmt.setString(4, params.getEmail());

            // SQL문 실행하기 --> 결과 행의 수를 리턴할 변수에 대입함
              result = pstmt.executeUpdate();
        } catch (SQLException e) {
            System.out.println("MySQL SQL Fail : " + e.getMessage());
        } finally {
            if (pstmt != null) {
                // 객체 닫기
                try {
                    pstmt.close();
                } catch (SQLException e) {}
            }
        }
        return result;
    }

}

```
