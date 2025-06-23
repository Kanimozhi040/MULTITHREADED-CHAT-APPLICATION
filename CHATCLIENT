package chatapp;
import java.io.*;
import java.net.*;

public class ChatClient {
    public static void main(String[] args) throws IOException {
        Socket socket = new Socket("localhost", 1234);
        System.out.println("Connected to the chat server.");

        new Thread(new ReceiveMessages(socket)).start();

        BufferedReader userInput = new BufferedReader(new InputStreamReader(System.in));
        PrintWriter out = new PrintWriter(socket.getOutputStream(), true);

        String message;
        while ((message = userInput.readLine()) != null) {
            out.println(message);
        }
    }
}

class ReceiveMessages implements Runnable {
    private BufferedReader in;

    public ReceiveMessages(Socket socket) throws IOException {
        in = new BufferedReader(new InputStreamReader(socket.getInputStream()));
    }

    public void run() {
        String response;
        try {
            while ((response = in.readLine()) != null) {
                System.out.println("Server: " + response);
            }
        } catch (IOException e) {
            System.out.println("Disconnected from server.");
        }
    }
}
