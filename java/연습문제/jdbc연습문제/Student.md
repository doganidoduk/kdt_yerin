```
public class Student {
    private int stdno;
    private String stdname;
    private String phone;
    private String email;
    
    public Student(int stdno, String stdname, String phone, String email) {
        this.stdno = stdno;
        this.stdname = stdname;
        this.phone = phone;
        this.email = email;
    }
    public int getStdno() {
        return stdno;
    }
    public void setStdno(int stdno) {
        this.stdno = stdno;
    }
    public String getStdname() {
        return stdname;
    }
    public void setStdname(String stdname) {
        this.stdname = stdname;
    }
    public String getPhone() {
        return phone;
    }
    public void setPhone(String phone) {
        this.phone = phone;
    }
    public String getEmail() {
        return email;
    }
    public void setEmail(String email) {
        this.email = email;
    }
    @Override
    public String toString() {
        return "Student [stdno=" + stdno + ", stdname=" + stdname + ", phone=" + phone + ", email=" + email + "]";
    }

    
}

```
