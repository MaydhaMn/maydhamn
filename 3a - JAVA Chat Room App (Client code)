import java.io.*;
import java.net.*;
import javax.swing.*;
import static java.lang.System.out;
import java.awt.*;
import java.awt.event.*;

public class Client extends JFrame implements ActionListener {
	String userName;
	Socket connection;
	PrintWriter p_writer;
	BufferedReader buff;
	JTextArea text_area_msgs;
	JTextField input;

	public Client(String userName, String servername) throws Exception {
		super(userName + "'s chat window"); // Frame title

		this.userName = userName;
		connection = new Socket(servername, 26714); // Make a socket connection
													// with assigned port number
													// and servername

		buff = new BufferedReader(new InputStreamReader(connection.getInputStream()));
		p_writer = new PrintWriter(connection.getOutputStream(), true);

		p_writer.println(userName); // Send the user name to the server

		BuildChatWindow();
		new MsgThread().start(); // this thread will listen for messages
	}

	public static void main(String args[]) {

		// Ask for a username
		String uname = JOptionPane.showInputDialog(null, "Enter your name: ", "Username", JOptionPane.PLAIN_MESSAGE);
		String join = JOptionPane.showInputDialog(null, "Type 'JOIN' to join the chat room ", "JOIN",
				JOptionPane.PLAIN_MESSAGE);
		String servername = "localhost";
		try {
			// only if the user types "JOIN" we will allow them to connect to
			// the server
			// and be recognized as an online client
			if (join.equals("JOIN")) {
				new Client(uname, servername);
			} else {
				JOptionPane.showMessageDialog(null, "BYE!");
			}
		} catch (Exception e) {
			out.println("Error: " + e.getMessage());
		}

	}

	public void BuildChatWindow() {

		// Simple GUI for Main chat area
		text_area_msgs = new JTextArea();
		text_area_msgs.setRows(10);
		text_area_msgs.setColumns(50);
		text_area_msgs.setEditable(false);
		JScrollPane chatPanel = new JScrollPane(text_area_msgs, JScrollPane.VERTICAL_SCROLLBAR_AS_NEEDED,
				JScrollPane.HORIZONTAL_SCROLLBAR_NEVER);
		JPanel jp1 = new JPanel(new FlowLayout());
		jp1.add(chatPanel);

		add(jp1, "Center");
		input = new JTextField(50); // where the user will give their input

		JPanel jp2 = new JPanel(new FlowLayout());
		jp2.add(input);
		add(jp2, "South");

		input.addActionListener(this);// user can simple press enter to send
										// msgs
		setSize(100, 50);
		setVisible(true);
		pack();

	}

	@Override
	public void actionPerformed(ActionEvent evt) {

		// If user types "LIST" show the list of online users
		if (input.getText().contains("LIST")) {
			p_writer.println("LIST");
			input.setText(""); // set text in input box back to null after
								// sending the msg
		}

		// If user types "LEAVE", exit!
		else if (input.getText().contains("LEAVE")) {
			p_writer.println("LEAVE");
			try {
				// Close socket connection!
				connection.close();
			} catch (IOException e) {
				e.printStackTrace();
			}
			System.exit(0);
		}

		// Otherwise continue to show the messages as usual in the chat window
		else {
			p_writer.println(input.getText());
			input.setText(""); // set text in input box back to null after
								// sending the msg
		}
	}

	class MsgThread extends Thread {

		@Override
		public void run() {
			String s;
			try {
				while (true) {
					s = buff.readLine();
					text_area_msgs.append(s + "\n");
					text_area_msgs.setCaretPosition(text_area_msgs.getDocument().getLength()); // automatically
																								// scroll
																								// to
																								// where
																								// the
																								// msgs
																								// are
				}
			} catch (Exception e) {
				out.println("Error: " + e.getMessage());
			}
		}
	}
}
