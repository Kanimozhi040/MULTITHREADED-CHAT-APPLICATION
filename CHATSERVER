package chatapp;
import java.io.*;
import java.net.*;
import java.util.*;
public class ChatServer
{
    private static Set<ClientHandler> clientHandlers = new HashSet<>();
    public static void main(String[] args) throws IOException 
    {
        ServerSocket serverSocket = new ServerSocket(1234);
        System.out.println("Server started. Waiting for clients...");
        while (true) 
        {
            Socket socket = serverSocket.accept();
            System.out.println("New client connected: " + socket);
            ClientHandler clientHandler = new ClientHandler(socket);
            clientHandlers.add(clientHandler);
            new Thread(clientHandler).start();
        }
    }
    public static void broadcast(String message, ClientHandler sender) 
    {
        for (ClientHandler client : clientHandlers) 
        {
            if (client != sender)
            {
                client.sendMessage(message);
            }
        }
    }

    public static void removeClient(ClientHandler clientHandler)
    {
        clientHandlers.remove(clientHandler);
    }
}
class ClientHandler implements Runnable 
{
    private Socket socket;
    private PrintWriter out;
    private BufferedReader in;

    public ClientHandler(Socket socket)
    {
        this.socket = socket;
    }

    public void sendMessage(String message) 
    {
        out.println(message);
    }

    public void run()
    {
        try 
        {
            in = new BufferedReader(new InputStreamReader(socket.getInputStream()));
            out = new PrintWriter(socket.getOutputStream(), true);

            out.println("Welcome to the chat!");

            String message;
            while ((message = in.readLine()) != null)
            {
                System.out.println("Received: " + message);
                ChatServer.broadcast(message, this);
            }
        }
        catch (IOException e) 
        {
            System.out.println("Client disconnected: " + socket);
        } finally
        {
            try 
            {
                socket.close();
            }
            catch (IOException e) 
            {
                e.printStackTrace();
            }
            ChatServer.removeClient(this);
        }
    }
}

