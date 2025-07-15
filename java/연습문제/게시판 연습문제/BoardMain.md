```
import java.awt.BorderLayout;
import java.awt.Color;
import java.awt.Component;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.awt.event.MouseAdapter;
import java.util.List;
import java.awt.event.MouseEvent;

import javax.swing.JButton;
import javax.swing.JFrame;
import javax.swing.JLabel;
import javax.swing.JOptionPane;
import javax.swing.JPanel;
import javax.swing.JScrollPane;
import javax.swing.JTable;
import javax.swing.table.DefaultTableModel;
import javax.swing.table.TableCellRenderer;

import boardapp.model.BoardDao;
import boardapp.model.BoardDto;
import boardapp.view.BoardMain.CenterTableCellRenderer;

public class BoardMain extends JFrame{
    private JTable jTable;
    private JPanel pSouth;
    private JButton btnInsert;
    
     public BoardMain() {
        setTitle("게시판 리스트");
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        getContentPane().add(new JScrollPane(getJTable()), BorderLayout.CENTER);
        getContentPane().add(getPSouth(), BorderLayout.SOUTH);
        setSize(600, 450);
        setLocationRelativeTo(null); // 화면 중앙 정렬
        setVisible(true);

        // 처음 실행 시 테이블 데이터 로딩
        refreshTable();
    }

      JTable getJTable() {
        if (jTable == null) {
            jTable = new JTable();

            DefaultTableModel tableModel = (DefaultTableModel) jTable.getModel();
            tableModel.addColumn("번호");
            tableModel.addColumn("제목");
            tableModel.addColumn("글쓴이");
            tableModel.addColumn("날짜");
            tableModel.addColumn("조회수");

            jTable.getColumn("번호").setPreferredWidth(20);
            jTable.getColumn("제목").setPreferredWidth(250);
            jTable.getColumn("글쓴이").setPreferredWidth(50);
            jTable.getColumn("날짜").setPreferredWidth(50);
            jTable.getColumn("조회수").setPreferredWidth(20);

            CenterTableCellRenderer ctcr = new CenterTableCellRenderer();
            jTable.getColumn("번호").setCellRenderer(ctcr);
            jTable.getColumn("제목").setCellRenderer(ctcr);
            jTable.getColumn("글쓴이").setCellRenderer(ctcr);
            jTable.getColumn("날짜").setCellRenderer(ctcr);
            jTable.getColumn("조회수").setCellRenderer(ctcr);

            // 나중에 클릭 시 글 상세 보기 등의 기능 추가 가능
            jTable.addMouseListener(new MouseAdapter() {
                public void mouseClicked(MouseEvent e) {
                 if (e.getClickCount() == 1) {
                int row = jTable.getSelectedRow();
                if (row != -1) {
                int boardNo = (int) jTable.getValueAt(row, 0); // 0번 열 = 글 번호

                
            try {
                BoardDao dao = new BoardDao();
                BoardDto dto = dao.selectByNo(boardNo);  // 번호로 게시글 정보 가져오기
                dao.close();

                // 게시글 보기 다이얼로그 열기
                ViewDialog dialog = new ViewDialog(BoardMain.this, dto);
                dialog.setVisible(true);
            } catch (Exception ex) {
                JOptionPane.showMessageDialog(BoardMain.this,
                    "게시글 불러오기 실패: " + ex.getMessage(),
                    "오류", JOptionPane.ERROR_MESSAGE);
                ex.printStackTrace();
            }
        }
            
        }
                }
            });
        }
        return jTable;
    }

    public class CenterTableCellRenderer extends JLabel implements TableCellRenderer {
        public Component getTableCellRendererComponent(JTable table, Object value, boolean isSelected,
                                                       boolean hasFocus, int row, int column) {
            setText(value.toString());
            setHorizontalAlignment(JLabel.CENTER);
            setOpaque(true);
            setBackground(isSelected ? Color.YELLOW : Color.WHITE);
            return this;
        }
    }

     public JPanel getPSouth() {
        if (pSouth == null) {
            pSouth = new JPanel();
            pSouth.add(getBtnInsert());
        }
        return pSouth;
    }
       public JButton getBtnInsert() {
        if (btnInsert == null) {
            btnInsert = new JButton("추가");
            btnInsert.addActionListener(new ActionListener() {
                public void actionPerformed(ActionEvent e) {
                    // 글 추가 다이얼로그 띄우기
                    InsertDialog dialog = new InsertDialog(BoardMain.this);
                    dialog.setVisible(true);
                }
            });
        }
        return btnInsert;
    }

    public void refreshTable() {
        try {     
            DefaultTableModel model = (DefaultTableModel) jTable.getModel();
            model.setRowCount(0); // 기존 데이터 삭제

            BoardDao dao = new BoardDao();
            List<BoardDto> list = dao.selectAll();
            dao.close();

            for (BoardDto dto : list) {
                Object[] row = {
                    dto.getNo(),
                    dto.getTitle(),
                    dto.getWriter(),
                    dto.getDate(),
                    dto.getHitcount()
                };
                model.addRow(row);
            }

        } catch (Exception e) {
            e.printStackTrace();
            JOptionPane.showMessageDialog(this, "데이터 불러오기 중 오류 발생: " + e.getMessage());
        }
    }

    public static void main(String[] args) {
        new BoardMain();
    }


    
}
```
