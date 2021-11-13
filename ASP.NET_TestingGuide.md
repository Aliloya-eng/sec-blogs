# ASP.NET Security Testing Techniques
- This blog is intended for ethical activities so please use it for ethical testing only.
- I wanted this blog to be an exhaustive list of all vulnerabilities and tricks that are mentioned on the web to be of a special focus in ASP.NET applications, I myself have seen some but not all of them. I wrote it as simple and as direct as possible to be available for everyone to use it, despite the level of experience, and I hope it would be of great help for anyone who needs a starting point to test .NET applications -whether you are a penetration tester or a bug bounty hunter.
- Any contribution is very appreciated, as well as any advice or comment.

==================================================================================
## ENUMERATION
No need for me to say how important is enumeration! I list herein _additional_ directories and extentions to look for in a .NET application.
### Directories
  - C:\Windows\System32
  - \\Host\C$\Windows\System32
  - \\.\C:\Windows\System32
  - \\?\C:\Windows\System32
  - \\?\UNC\Host\C$\Windows\System32
  - C:\Windows\hh.exe == C:\Windows:$I30:$INDEX_ALLOCATION\hh.exe
  - C:\Windows\notepad.exe == C:\Windows\notepad.exe::$DATA
  - FileName.aspx == FileName.aspx:.jpg
  - http://hostname/*Resource.axd?d=<resourceId\>&t=\<timestamp\>

Also take a look at the following reminders before you start your directory bruteforce:
  - Reserved: NUL, CON, AUX, PRN, COM, LPT
  - Endings: name == name… == name \\\
  - Case insensitivity
### Files Extensions
  - .asp
  - .aspx
  - .ashx
  - .xsd
  - .disco
  - .discomap
  - .wsdl
  - .wadl
  - .asmx
  - .xml
  - .config
  - .zip
  - .txt
  
## TECHNIQUES AND TRICKS
I wanted this list to be an exhaustive list of all vulnerabilities that are mentioned on the web to be of a special focus in ASP.NET applications, I myself have seen some and not all of them, and I guess they are common among .NET in general -you will recognize some of them to be valid for all types of backends too but I was not as keen to mention all of those.
### Short Names (IIS tilde bug)
The vulnerability is caused by a tilde character "~" in a GET or OPTIONS request, which could allow remote attackers to disclose 8.3 filenames.
[IIS-ShortName-Scanner](https://github.com/irsdl/IIS-ShortName-Scanner). Command: `java –jar iis_shortname_scanner.jar [2=Show_Progress] [20=Threads] [Target_URL] > [OutFile]`
<br>PS. ignore “global”, “master”, “default”, “aspnet” from results.
### Viewstate
The ViewState is a mechanism built into the ASP.NET platform for persisting elements of the user interface and other data across successive requests. The data to be persisted is serialized by the server and transmitted via a hidden form field. When it is posted back to the server, the ViewState parameter is deserialized and the data is retrieved. An attacker can modify the contents of the ViewState and cause arbitrary data to be deserialized and processed by the server. An attacker may be able to execute arbitrary code on the server by supplying a gadget chain. Also, if the ViewState contains any items that are critical to the server's processing of the request, then this may result in a direct security exposure.
Looking further into this vulnerability you would notice that it has many possible variations. As a simple first step, you can start by testing if the ViewState is not encrypted simply by finding any ViewState decoder on the web (e.g. http://viewstatedecoder.azurewebsites.net/)
### Remote debugging
Since ASP.NET is a compiled application, it has certain debugging features. Microsoft allows you to use a remote debugger to work on a debug version of the application.  If this port is open on the Internet and is protected by a simple password, or there is no password at all, then it is possible to pick up a debugger session.
### SMTP Header Injection
No validation of the value that falls in the “To” header, it is possible to redirect the letter to another recipient. But that would be too simple and obvious, so validation occurs already at the .Net level. However, if you introduce a new Reply-To header — the answer address, many forms such as “Forgotten Password” often take the sending address from it, thus it is enough to embed carriage return and line feed characters and get the workload.

>From: mail@mail.mail (R.T. Hood)<br>
>To: mail@mail.mail/r/nReply-to:mail@mail.mail<br>
>Date: Tue, Mar 18199714:36:14 PST<br>
>Message-Id: <mail@mail.mail><br>
>...<br>
>В итоге будет внедрен новый заголовок:<br>
>From: mail@mail.mail (R.T. Hood)<br>
>To: mail@mail.mail<br>
>Reply-To: mail@mail.mail<br>
>Date: Tue, Mar 18199714:36:14 PST<br>
>Message-Id: <mail@mail.mail><br>
### Additional .NET-Specific Tricks
1. Canonicalization attack
   - Using environment variables to represent path - e.g. %windir%notepad.exe
   - Trailing “.” - e.g. C:wondowsnotepad.exe.
   - Create file - \\URL\<name>		\\URL\pipe\<name>
2. LINQ injection
3. Hash collusion: https://github.com/HybrisDisaster/aspHashDoS
### Not-.NET-Specific
1. XXE in DocX files
2. Padding Oracle (cookies / ViewState …)
3. Injection (SQL with SQLNinja, js, HTML…)
4. Format strings attack - e.g %n%n%n%n%n%n%n%n  ||  %s%s%s%s%s%s%s%s%s  ||  %d%d%d%d%d%d%d%d
5. Serialization
6. LFI - simple webshell `<%@ Page Language="C#" %> <%@ Import Namespace="System.Diagnostics" %> <%= Process.Start( new ProcessStartInfo( "cmd","/c " + Request["c"] ) { UseShellExecute = false, RedirectStandardOutput = true } ).StandardOutput.ReadToEnd() %>`
7. Trace method
8. Session fixation 
9. Replay attacks
10. CORS
 
## References
- https://www.youtube.com/watch?v=HrJW6Y9kHC4 
- https://cheatsheetseries.owasp.org/cheatsheets/DotNet_Security_Cheat_Sheet.html
- https://resources.infosecinstitute.com/topic/net-penetration-testing-test-case-cheat-sheet/
- https://www.business2community.com/cybersecurity/9-ways-hackers-exploit-asp-net-and-how-to-prevent-them-02353604
- Positive technologies [PDF](https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=&cad=rja&uact=8&ved=2ahUKEwiR5K7i-5T0AhUKh_0HHQM2CW0QFnoECAMQAQ&url=https%3A%2F%2Fwww.ptsecurity.com%2Fupload%2Fcorporate%2Fru-ru%2Fwebinars%2Fics%2FV.Kochetkov_breaking_ASP.NET.pdf&usg=AOvVaw1BQOnt5w4XQizf5Tosp201)
