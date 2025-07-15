```
import javax.swing.*;
import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;

import boardapp.model.BoardDao;
import boardapp.model.BoardDto;


public class UpdateDialog extends JDialog{
     private BoardMain boardMain;
    private BoardDto dto;

    private JTextField txtTitle;
    private JTextField txtWriter;
    private JTextArea txtContent;
    private JButton btnUpdate, btnCancel;


    public UpdateDialog(BoardMain boardMain, BoardDto dto) {
        super(boardMain, "게시글 수정", true);
        this.boardMain = boardMain;
        this.dto = dto;

        setSize(400, 300);
        setLocationRelativeTo(boardMain);
        setLayout(new BorderLayout(10, 10));

        add(createCenterPanel(), BorderLayout.CENTER);
        add(createButtonPanel(), BorderLayout.SOUTH);

        // 기존 데이터 채우기
        txtTitle.setText(dto.getTitle());
        txtWriter.setText(dto.getWriter());
        txtContent.setText(dto.getContent());
    }

     private JPanel createCenterPanel() {
        JPanel panel = new JPanel(new GridLayout(3, 1, 5, 5));
        panel.setBorder(BorderFactory.createEmptyBorder(10, 10, 0, 10));

        JPanel p1 = new JPanel(new BorderLayout(5, 5));
        JLabel lbl1 = new JLabel("제목");
        txtTitle = new JTextField();
        p1.add(lbl1, BorderLayout.WEST);
        p1.add(txtTitle, BorderLayout.CENTER);

        JPanel p2 = new JPanel(new BorderLayout(5, 5));
        JLabel lbl2 = new JLabel("글쓴이");
        txtWriter = new JTextField();
        txtWriter.setEditable(false); // 글쓴이는 수정 안되도록
        p2.add(lbl2, BorderLayout.WEST);
        p2.add(txtWriter, BorderLayout.CENTER);

        JPanel p3 = new JPanel(new BorderLayout(5, 5));
        JLabel lbl3 = new JLabel("내용");
        txtContent = new JTextArea(5, 20);
        txtContent.setLineWrap(true);
        JScrollPane scrollPane = new JScrollPane(txtContent);
        p3.add(lbl3, BorderLayout.WEST);
        p3.add(scrollPane, BorderLayout.CENTER);

        panel.add(p1);
        panel.add(p2);
        panel.add(p3);

        return panel;
    }

        private JPanel createButtonPanel() {
        JPanel panel = new JPanel();
        btnUpdate = new JButton("수정");
        btnCancel = new JButton("취소");

        panel.add(btnUpdate);
        panel.add(btnCancel);

        // 수정 버튼 클릭 이벤트
        btnUpdate.addActionListener(new ActionListener() {
            public void actionPerformed(ActionEvent e) {
                String title = txtTitle.getText().trim();
                String content = txtContent.getText().trim();

                if (title.isEmpty() || content.isEmpty()) {
                    JOptionPane.showMessageDialog(UpdateDialog.this, "제목과 내용을 모두 입력하세요.");
                    return;
                }

                // dto에 반영
                dto.setTitle(title);
                dto.setContent(content);
                try {
                    BoardDao dao = new BoardDao();
                    int result = dao.delete(dto.getNo());
                    dao.close();
               
                if (result > 0) {
                    JOptionPane.showMessageDialog(UpdateDialog.this, "수정 성공!");
                    boardMain.refreshTable(); // 테이블 새로고침
                    dispose();
                } else {
                    JOptionPane.showMessageDialog(UpdateDialog.this, "수정 실패...");
                }
            } catch (Exception o) {

            }
            }
        });

        btnCancel.addActionListener(e -> dispose());

        return panel;
    }
    
}
```
