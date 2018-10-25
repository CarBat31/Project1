# Project1
Connect 4 Game

package Project1;


import java.awt.*;

	public class Connect4 {

		
		public static void main(String[] args) {
			Connect4JFrame frame = new Connect4JFrame();
			frame.setDefaultCloseOperation( JFrame.EXIT_ON_CLOSE);
			frame.setVisible(true);
		}
	}

	class Connect4JFrame extends JFrame implements ActionListener {

		private JButton		btn1, btn2, btn3, btn4, btn5, btn6, btn7;
		private JLabel		lblSpacer;
		MenuItem		newMI, exitMI, blackMI, whiteMI;
		int[][]			theArray;
		boolean			end=false;
		boolean			gameStart;
		public static final int BLANK = 0;
		public static final int BLACK = 1;
		public static final int WHITE = 2;

		public static final int MAXROW = 6;	
		public static final int MAXCOL = 7;	

		public static final String SPACE =   "                "; 

		int activeColor = BLACK;
		
		public Connect4JFrame() {
			
			setTitle("Connect 4 Game");
			MenuBar mbar = new MenuBar();
			Menu GameMenu = new Menu("Game");
			
			newMI = new MenuItem("New");
			newMI.addActionListener(this);
			GameMenu.add(newMI);
			exitMI = new MenuItem("Exit");
			exitMI.addActionListener(this);
			GameMenu.add(exitMI);
			mbar.add(GameMenu);
			Menu optMenu = new Menu("Options");
			blackMI = new MenuItem("P1_Black starts");
			blackMI.addActionListener(this);
			optMenu.add(blackMI);
			whiteMI = new MenuItem("P2_White starts");
			whiteMI.addActionListener(this);
			optMenu.add(whiteMI);
			mbar.add(optMenu);
			setMenuBar(mbar);
			

			// control panel buttons.
			Panel panel = new Panel();

			btn1 = new JButton("1");
			btn1.addActionListener(this);
			panel.add(btn1);
			lblSpacer = new JLabel(SPACE);
			panel.add(lblSpacer);

			btn2 = new JButton("2");
			btn2.addActionListener(this);
			panel.add(btn2);
			lblSpacer = new JLabel(SPACE);
			panel.add(lblSpacer);

			btn3 = new JButton("3");
			btn3.addActionListener(this);
			panel.add(btn3);
			lblSpacer = new JLabel(SPACE);
			panel.add(lblSpacer);

			btn4 = new JButton("4");
			btn4.addActionListener(this);
			panel.add(btn4);
			lblSpacer = new JLabel(SPACE);
			panel.add(lblSpacer);

			btn5 = new JButton("5");
			btn5.addActionListener(this);
			panel.add(btn5);
			lblSpacer = new JLabel(SPACE);
			panel.add(lblSpacer);

			btn6 = new JButton("6");
			btn6.addActionListener(this);
			panel.add(btn6);
			lblSpacer = new JLabel(SPACE);
			panel.add(lblSpacer);

			btn7 = new JButton("7");
			btn7.addActionListener(this);
			panel.add(btn7);

			add(panel, BorderLayout.NORTH);
			initialize();
			
			setSize(1000, 750);   
		} 

		public void initialize() {
			theArray=new int[MAXROW][MAXCOL];
			for (int row=0; row<MAXROW; row++)
				for (int col=0; col<MAXCOL; col++)
					theArray[row][col]=BLANK;
			gameStart=false;
		} // initialize Board

		public void paint(Graphics g) {
			g.setColor(Color.BLUE);
			g.fillRect(110, 50, 100+100*MAXCOL, 100+100*MAXROW);
			for (int row=0; row<MAXROW; row++)
				for (int col=0; col<MAXCOL; col++) {
					if (theArray[row][col]==BLANK) g.setColor(Color.YELLOW);
					if (theArray[row][col]==BLACK) g.setColor(Color.BLACK);
					if (theArray[row][col]==WHITE) g.setColor(Color.WHITE);
					g.fillOval(160+100*col, 100+100*row, 100, 100);
				}
			check4(g);
		} 

		public void putChip(int n) {   // put a chip on top of column n
		
			// if game is won, do nothing
			if (end) return;
			gameStart=true;
			int row;
			n--;
			for (row=0; row<MAXROW; row++)
				if (theArray[row][n]>0) break;
			if (row>0) {
				theArray[--row][n]=activeColor;
				if (activeColor==BLACK)
					activeColor=WHITE;
				else
					activeColor=BLACK;
				repaint();
			}
		}

		public void displayWinner(Graphics g, int n) {
			g.setColor(Color.BLACK);
			g.setFont(new Font("Courier", Font.BOLD, 100));
			if (n==BLACK)
				g.drawString("Black wins!", 100, 200);
			
			else
				g.setColor(Color.WHITE);
			    g.setFont(new Font("Courier", Font.BOLD, 100));
			if (n==WHITE)

				g.drawString("White wins!", 100, 200);
			end=true;
		}
				
		public void check4(Graphics g) {    // see if there are 4 disks in a row: horizontal, vertical or diagonal
		
			
			// horizontal rows
			for (int row=0; row<MAXROW; row++) {
				for (int col=0; col<MAXCOL-3; col++) {
					int curr = theArray[row][col];
					if (curr>0
					 && curr == theArray[row][col+1]
					 && curr == theArray[row][col+2]
					 && curr == theArray[row][col+3]) {
						displayWinner(g, theArray[row][col]);
					}
				}
			}
			// vertical 
			for (int col=0; col<MAXCOL; col++) {
				for (int row=0; row<MAXROW-3; row++) {
					int curr = theArray[row][col];
					if (curr>0
					 && curr == theArray[row+1][col]
					 && curr == theArray[row+2][col]
					 && curr == theArray[row+3][col])
						displayWinner(g, theArray[row][col]);
				}
			}
			// diagonal lower left to upper right
			for (int row=0; row<MAXROW-3; row++) {
				for (int col=0; col<MAXCOL-3; col++) {
					int curr = theArray[row][col];
					if (curr>0
					 && curr == theArray[row+1][col+1]
					 && curr == theArray[row+2][col+2]
					 && curr == theArray[row+3][col+3])
						displayWinner(g, theArray[row][col]);
				}
			}
			// diagonal upper left to lower right
			for (int row=MAXROW-1; row>=3; row--) {
				for (int col=0; col<MAXCOL-3; col++) {
					int curr = theArray[row][col];
					if (curr>0
					 && curr == theArray[row-1][col+1]
					 && curr == theArray[row-2][col+2]
					 && curr == theArray[row-3][col+3])
						displayWinner(g, theArray[row][col]);
				}
			}
		} // end check4

		public void actionPerformed(ActionEvent e) {
			
			if (e.getSource() == btn1)
				putChip(1);
			else if (e.getSource() == btn2)
				putChip(2);
			else if (e.getSource() == btn3)
				putChip(3);
			else if (e.getSource() == btn4)
				putChip(4);
			else if (e.getSource() == btn5)
				putChip(5);
			else if (e.getSource() == btn6)
				putChip(6);
			else if (e.getSource() == btn7)
				putChip(7);
			else if (e.getSource() == newMI) {
				end=false;
				initialize();
				repaint();
			} else if (e.getSource() == exitMI) {
				System.exit(0);
			} else if (e.getSource() == blackMI) {
				
				if (!gameStart) activeColor=BLACK;
			} else if (e.getSource() == whiteMI) {
				if (!gameStart) activeColor=WHITE;
			}
		} // end ActionPerformed

	} 

	
