"# umair-sabre" //server
#include<stdio.h>
#include<netinet/in.h>
#include<sys/types.h>
#include<sys/socket.h>
#include<netdb.h>
#include<string.h>
#include<stdlib.h>
#include<unistd.h>
#include<arpa/inet.h
#define MAX 80
#define PORT 43454
#define SA struct sockaddr
void func(int sockfd)
{
char buff[MAX];
int n;
for(;;)
{
bzero(buff,sizeof(buff));
printf("Enter the string : ");
n=0;
while((buff[n++]=getchar())!='\n');
write(sockfd,buff,sizeof(buff));
bzero(buff,sizeof(buff));
read(sockfd,buff,sizeof(buff));
printf("From Server : %s",buff);
if((strncmp(buff,"assalamualaikum",4))==0)
{
printf("waalaikumsalam dah bye\n");
break;
}
}
}

int main()
{
int sockfd,connfd;
struct sockaddr_in servaddr,cli;
sockfd=socket(AF_INET,SOCK_STREAM,0);
if(sockfd==-1)
{
printf("socket creation failed...\n");
exit(0);
}
else
printf("Socket successfully created..\n");
bzero(&servaddr,sizeof(servaddr));
servaddr.sin_family=AF_INET;
servaddr.sin_addr.s_addr=inet_addr("192.168.27.131");
servaddr.sin_port=htons(PORT);
if(connect(sockfd,(SA *)&servaddr,sizeof(servaddr))!=0)
{
printf("connection with the server failed...\n");
exit(0);
}
else
printf("connected to the server..\n");
func(sockfd);
close(sockfd);
}
//client
#include<stdio.h>
#include<netinet/in.h>
#include<sys/types.h>
#include<sys/socket.h>
#include<netdb.h>
#include<stdlib.h>
#include<string.h>
#define MAX 80
#define PORT 43454
#define SA struct sockaddr
void func(int sockfd)
{
char buff[MAX];
int n;
for(;;)
{
bzero(buff,MAX);
read(sockfd,buff,sizeof(buff));
printf("From client: %s\t To client : ",buff);
bzero(buff,MAX);
n=0;
while((buff[n++]=getchar())!='\n');
write(sockfd,buff,sizeof(buff));
if(strncmp("assalamualaikum",buff,4)==0)
{
printf("Server Exit...\n");
break;
}
}
}
int main()
{
int sockfd,connfd,len;
struct sockaddr_in servaddr,cli;
sockfd=socket(AF_INET,SOCK_STREAM,0);
if(sockfd==-1)
{
printf("socket creation failed...\n");
exit(0);
}
else
printf("Socket successfully created..\n");
bzero(&servaddr,sizeof(servaddr));
servaddr.sin_family=AF_INET;
servaddr.sin_addr.s_addr=htonl(INADDR_ANY);
servaddr.sin_port=htons(PORT);
if((bind(sockfd,(SA*)&servaddr, sizeof(servaddr)))!=0)
{
printf("socket bind failed...\n");
exit(0);
}
else
printf("Socket successfully binded..\n");
if((listen(sockfd,5))!=0)
{
printf("Listen failed...\n");
exit(0);
}
else
printf("Server listening..\n");
len=sizeof(cli);
connfd=accept(sockfd,(SA *)&cli,&len);
if(connfd<0)
{
printf("server acccept failed...\n");
exit(0);
}
else
printf("server acccept the client...\n");
func(connfd);
close(sockfd);
}
//java socket timeout
import java.io.IOException;
	import java.net.InetAddress;
import java.net.InetSocketAddress;
	import java.net.Socket;
	import java.net.SocketAddress;
	import java.net.UnknownHostException;
	public class CreateClientSocketWithTimeout {
	
	    public static void main(String[] args) {
	 try {
	            InetAddress addr = InetAddress.getByName("javacodegeeks.com");
	            int port = 80;
	            SocketAddress sockaddr = new InetSocketAddress(addr, port);
	            // Creates an unconnected socket
	            Socket socket = new Socket();
	 
	            int timeout = 5000;   // 5000 millis = 5 seconds
	             
	            // Connects this socket to the server with a specified timeout value
	            // If timeout occurs, SocketTimeoutException is thrown
	            socket.connect(sockaddr, timeout);
	             
	            System.out.println("Socket connected..." + socket);
	             
	        }
	        catch (UnknownHostException e) {
	            System.out.println("Host not found: " + e.getMessage());
	        }
	        catch (IOException ioe) {
	            System.out.println("I/O Error " + ioe.getMessage());
	        }
	         
	   }
	}

