```
import java.awt.BorderLayout;
import java.awt.Color;
import java.awt.Dimension;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;

import javax.swing.JButton;
import javax.swing.JDialog;
import javax.swing.JLabel;
import javax.swing.JOptionPane;
import javax.swing.JPanel;
import javax.swing.JTextArea;
import javax.swing.JTextField;
import javax.swing.WindowConstants;
import javax.swing.border.EmptyBorder;
import javax.swing.border.EtchedBorder;

import boardapp.model.BoardDao;
import boardapp.model.BoardDto;

public class InsertDialog extends JDialog{
    private BoardMain boardMain;
	private JPanel pCenter, pTitle, pContent, pWriter, pSouth;
	private JTextField txtTitle, txtWriter;
	private JTextArea txtContent;
	private JButton btnOk, btnCancel;
	
	public InsertDialog(BoardMain boardMain) {
		super(boardMain);
		this.boardMain = boardMain;
		this.setTitle("게시물 작성");					
		this.setDefaultCloseOperation(WindowConstants.DISPOSE_ON_CLOSE);
		this.setResizable(false);	
		this.setModal(true);
		this.setSize(400, 270);
		
		this.setLocation(
			boardMain.getLocationOnScreen().x + (boardMain.getWidth()-this.getWidth())/2,
			boardMain.getLocationOnScreen().y + (boardMain.getHeight()-this.getHeight())/2
		);
		
		this.getContentPane().add(getPCenter(), BorderLayout.CENTER);
		this.getContentPane().add(getPSouth(), BorderLayout.SOUTH);
	}
	public JPanel getPCenter() {
		if(pCenter==null) {
			pCenter = new JPanel();
			pCenter.setBorder(new EmptyBorder(10,10,10,10));
			pCenter.add(getPTitle());
			pCenter.add(getPWriter());
			pCenter.add(getPContent());
		}
		return pCenter;
	}
	
	public JPanel getPTitle() {
		if(pTitle==null) {
			pTitle = new JPanel();
			pTitle.setLayout(new BorderLayout());
			JLabel label = new JLabel("제목");
			label.setAlignmentX(JLabel.CENTER);
			label.setPreferredSize(new Dimension(70, 30));
			label.setHorizontalAlignment(JLabel.CENTER);
			pTitle.add(label, BorderLayout.WEST);
			txtTitle = new JTextField();
			txtTitle.setPreferredSize(new Dimension(300, 30));
			pTitle.add(txtTitle, BorderLayout.CENTER);
		}
		return pTitle;
	}	
	
	public JPanel getPWriter() {
		if(pWriter==null) {
			pWriter = new JPanel();
			pWriter.setLayout(new BorderLayout());
			JLabel label = new JLabel("글쓴이");
			label.setAlignmentX(JLabel.CENTER);
			label.setPreferredSize(new Dimension(70, 30));
			label.setHorizontalAlignment(JLabel.CENTER);
			pWriter.add(label, BorderLayout.WEST);
			txtWriter = new JTextField();
			txtWriter.setPreferredSize(new Dimension(300, 30));
			pWriter.add(txtWriter, BorderLayout.CENTER);
		}
		return pWriter;
	}		
	
	public JPanel getPContent() {
		if(pContent == null) {
			pContent = new JPanel();
			pContent.setLayout(new BorderLayout());
			JLabel label = new JLabel("내용");
			label.setAlignmentX(JLabel.CENTER);
			label.setPreferredSize(new Dimension(70, 30));
			label.setHorizontalAlignment(JLabel.CENTER);
			pContent.add(label, BorderLayout.WEST);
			txtContent = new JTextArea();
			txtContent.setBorder(new EtchedBorder());
			txtContent.setPreferredSize(new Dimension(300, 100));
			pContent.add(txtContent, BorderLayout.CENTER);
		}
		return pContent;
	}
	
	public JPanel getPSouth() {
		if(pSouth == null) {
			pSouth = new JPanel();
			pSouth.setBackground(Color.WHITE);
			pSouth.add(getBtnOk());
			pSouth.add(getBtnCancel());
		}
		return pSouth;
	}
	
	public JButton getBtnOk() {
		if(btnOk == null) {
			btnOk = new JButton();
			btnOk.setText("저장");
			btnOk.addActionListener(new ActionListener() {
				@Override
				public void actionPerformed(ActionEvent e) {
				try {
                // 입력값 읽기
                String title = txtTitle.getText().trim();
                String writer = txtWriter.getText().trim();
                String content = txtContent.getText().trim();

                // 빈 값 검사
                if (title.isEmpty() || writer.isEmpty() || content.isEmpty()) {
                JOptionPane.showMessageDialog(InsertDialog.this, "모든 항목을 입력해주세요.");
                return;
                }
                BoardDto board = new BoardDto();
                board.setTitle(title);
                board.setWriter(writer);
                board.setContent(content);

                BoardDao dao = new BoardDao();
                dao.insert(board);
                dao.close();

                JOptionPane.showMessageDialog(InsertDialog.this, "게시글이 저장되었습니다.");
                dispose();

                if (boardMain instanceof BoardMain) {
                ((BoardMain) boardMain).refreshTable(); // 나중에 추가할 메서드
                }
                 } catch (Exception ex) {
                ex.printStackTrace();
                JOptionPane.showMessageDialog(InsertDialog.this, "저장 중 오류 발생: " + ex.getMessage());
                }

        

			}

			});
		}
		return btnOk;
	}
	
	public JButton getBtnCancel() {
		if(btnCancel == null) {
			btnCancel = new JButton();
			btnCancel.setText("취소");
			btnCancel.addActionListener(new ActionListener() {
				@Override
				public void actionPerformed(ActionEvent e) {
					dispose();
				}
			});
		}
		return btnCancel;
	}	

    
}
```
