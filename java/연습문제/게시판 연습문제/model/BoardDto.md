```
public class BoardDto {
   private int no;              // 글 번호 (PK)
    private String title;        // 제목
    private String writer;       // 작성자
    private String content;      // 내용
    private String date;         // 작성일 (문자열로 처리)
    private int hitcount;        // 조회수





    public BoardDto() {}
   
     public BoardDto(int no, String title, String writer, String content, String date, int hitcount) {
        this.no = no;
        this.title = title;
        this.writer = writer;
        this.content = content;
        this.date = date;
        this.hitcount = hitcount;
    }

   
public int getNo() {
        return no;
    }
    public void setNo(int no) {
        this.no = no;
    }

    public String getTitle() {
        return title;
    }
    public void setTitle(String title) {
        this.title = title;
    }

    public String getWriter() {
        return writer;
    }
    public void setWriter(String writer) {
        this.writer = writer;
    }

    public String getContent() {
        return content;
    }
    public void setContent(String content) {
        this.content = content;
    }

    public String getDate() {
        return date;
    }
    public void setDate(String date) {
        this.date = date;
    }

    public int getHitcount() {
        return hitcount;
    }
    public void setHitcount(int hitcount) {
        this.hitcount = hitcount;
    }
    
}
```
