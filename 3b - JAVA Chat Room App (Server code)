import java.io.*;
import java.net.*;
import java.util.*;
import static java.lang.System.out;

public class Server {

	// List of all clients
	static List<ManageClient> online_users = new ArrayList<ManageClient>();
	// List of all online users
	static List<String> client_list = new ArrayList<String>();

	ServerSocket connection;

	static int size;

	// main method
	public static void main(String... args) throws Exception {

		new Server().connect();
	}

	public void connect() throws Exception {

		// Create a new server socket and wait until a Client joins so we can
		// connect with them

		int PORT = 26714;
		connection = new ServerSocket(26714);
		String inet = connection.getInetAddress().getHostName();
		out.println("Server Started..."); // Display this message while waiting
											// for the client to join

		while (true) {

			Socket client = connection.accept();
			ManageClient hc = new ManageClient(client);
			online_users.add(hc); // once the client joins in, add them to the
									// list

		}
	}

	public void broadcast(String client, String text) {
		for (ManageClient c : online_users) {
			c.sendMessage(client, text);
		}
	}

	public void getSize() {
		broadcast("\nOnline clients ", Integer.toString(size));
	}

	class ManageClient extends Thread {

		String name = ""; // client name
		BufferedReader input; // client's input
		PrintWriter output; // output to send to client

		public ManageClient(Socket socket) throws Exception {

			// setup input and output streams to send/recieve msgs
			input = new BufferedReader(new InputStreamReader(socket.getInputStream()));
			output = new PrintWriter(socket.getOutputStream(), true);

			name = input.readLine();
			client_list.add(name); // add to users list
			broadcast(name, " Has connected!");
			size++;
			start();

		}

		// Display the client's username with their msg in the chat window
		public void sendMessage(String uname, String msg) {
			output.println(uname + ": " + msg);
		}

		// Get the list of all online users
		public void get_online_users() {
			for (ManageClient c : online_users) {
				broadcast("Name", c.name);
				broadcast("Inet", connection.getInetAddress().getHostName());
				broadcast("PORT", Integer.toString(connection.getLocalPort()));
				broadcast("--------", "--------");
			}
			getSize();
		}

		public void run() {
			String line;

			try {
				while (true) {
					line = input.readLine(); // read input from the client

					// If client types "LEAVE" then disconnect them
					// and remove from the list
					if (line.equals("LEAVE")) {
						// notify all for user disconnection
						broadcast(name, " left the chat!");
						client_list.remove(this);
						online_users.remove(this);
						size--;

						break;
					}
					// If client types "LIST" then display all online users
					else if (line.equals("LIST")) {
						get_online_users();

					} else {// display the messages in each client's chat window
						broadcast(name, line);
					}
				}

			} catch (Exception ex) {
				System.out.println(ex.getMessage());
			}
		}
	}
}
