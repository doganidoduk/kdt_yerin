```
import javax.swing.*;
import java.awt.*;

import com.mysql.cj.x.protobuf.MysqlxNotice.Frame;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;

import boardapp.model.BoardDao;
import boardapp.model.BoardDto;

public class ViewDialog extends JDialog{
    private BoardMain boardMain;
    private BoardDto dto;
    private JTextField txtTitle;
    private JTextField txtWriter;
    private JTextArea txtContent;
    private JButton btnEdit, btnDelete, btnClose;

    public ViewDialog(BoardMain owner, BoardDto dto) {
        super(owner, "게시글 보기", true);
        this.dto = dto;
        this.boardMain = owner;

        setSize(400, 350);
        setLocationRelativeTo(owner);
      

        JPanel panel = new JPanel(new BorderLayout(10, 10));
        panel.setBorder(BorderFactory.createEmptyBorder(10,10,10,10));

        JPanel topPanel = new JPanel(new GridLayout(2,2,5,5));
        topPanel.add(new JLabel("제목"));
        txtTitle = new JTextField(dto.getTitle());
        txtTitle.setEditable(false);
        topPanel.add(txtTitle);

        topPanel.add(new JLabel("글쓴이"));
        txtWriter = new JTextField(dto.getWriter());
        txtWriter.setEditable(false);
        topPanel.add(txtWriter);

        txtContent = new JTextArea(dto.getContent());
        txtContent.setEditable(false);
        txtContent.setLineWrap(true);
        JScrollPane scrollPane = new JScrollPane(txtContent);

         JPanel buttonPanel = new JPanel();
        btnEdit = new JButton("수정");
        btnDelete = new JButton("삭제");
        btnClose = new JButton("닫기");

        buttonPanel.add(btnEdit);
        buttonPanel.add(btnDelete);
        buttonPanel.add(btnClose);

        panel.add(topPanel, BorderLayout.NORTH);
        panel.add(scrollPane, BorderLayout.CENTER);
        panel.add(buttonPanel, BorderLayout.SOUTH);

        add(panel);

         btnClose.addActionListener(e -> dispose());

        btnDelete.addActionListener(new ActionListener() {
    public void actionPerformed(ActionEvent e) {
        try {
            int confirm = JOptionPane.showConfirmDialog(
                ViewDialog.this,
                "정말 삭제하시겠습니까?",
                "삭제 확인",
                JOptionPane.YES_NO_OPTION
            );

            if (confirm == JOptionPane.YES_OPTION) {
                BoardDao dao = new BoardDao();
                int result = dao.delete(dto.getNo());
                dao.close();

                if (result > 0) {
                    JOptionPane.showMessageDialog(ViewDialog.this, "삭제 성공!");
                    boardMain.refreshTable();
                    dispose();
                } else {
                    JOptionPane.showMessageDialog(ViewDialog.this, "삭제 실패...");
                }
            }
        } catch (Exception ex) {
            ex.printStackTrace();
            JOptionPane.showMessageDialog(ViewDialog.this, "오류 발생: " + ex.getMessage());
        }
    }
});



         btnEdit.addActionListener(new ActionListener() {
            public void actionPerformed(ActionEvent e) {
                // 수정 다이얼로그 띄우기
                UpdateDialog editDialog = new UpdateDialog(boardMain, dto);
                editDialog.setVisible(true);
                dispose(); // 수정하고 이 창은 닫기
            }
        });

    }
    
   
}
```
