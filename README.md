<link rel ="stylesheet" href = "./README.css">

<h1>Git 배우기 1</h1>
<h2>특징</h2>
<ul>
<li>차이 대신 스냅샷 활용</li>
<img src = './git_img/snapshots.png'>
<li>거의 모든 명령 로컬 실행</li>
<li>무결성(check sum + SHA-1 기반)</li>
<li>데이터는 추가되기만(일단 commit하면 해당 정보는 잃어버리기 어려움 + 쉽게 복구 가능)</li>
</ul>

<h3>세가지 상태</h3>
<ul>
<li>Committed : 데이터가 로컬 데이터베이스 내에 저장된 상태</li>
<li>Modified : 수정된 파일을 아직 커밋 안한 상태</li>
<li>Staged : 수정된 파일을 커밋할 것이라고 표시한 상태</li>
</ul>
<img src = './git_img/areas.png'>
<h3>세가지 단계에 대한 이해</h3>
<ul>
<li>git 디렉토리 : 프로젝트 내 메타데이터/객체/데이터 저장소. 타 컴퓨터 저장소 clone시 디렉토리 생성</li>
<li>워킹트리 : 특정 버전을 checkout한 것. git 디렉토리는 현재 작업중 디스크에 있고, 그 디렉토리 안에 압축된 데이터베이스에서 파일을 가져와 워킹트리를 만든다.</li>
<li>staging area : 곧 커밋할 파일에 대한 정보를 저장.</li>
</ul>
<h3>git 이 하는 일</h3>
<ul>
<li>워킹 트리에서 파일 수정</li>
<li>staging area에 파일을 stage, 커밋할 스냅샷 만든다.(선택적 추가 가능)</li>
<li>staging area 내 파일을 커밋, git 디렉토리에 영구적 snapshot 저장</li>
</ul>


<h2>사용자 정보 변경</h2>
<pre>
$ git config --global user.name "John Doe"
$ git config --global user.email johndoe@example.com
$ git config --global core.editor vscode
</pre>
<p>
--global 옵션은 단 한번만 적용하면 모든 프로젝트에 적용.

/etc/gitconfig 파일: 시스템의 모든 사용자와 모든 저장소에 적용되는 설정이다. git config --system 옵션으로 이 파일을 읽고 쓸 수 있다. (이 파일은 시스템 전체 설정파일이기 때문에 수정하려면 시스템의 관리자 권한이 필요하다.)

~/.gitconfig, ~/.config/git/config 파일: 특정 사용자(즉 현재 사용자)에게만 적용되는 설정이다. git config --global 옵션으로 이 파일을 읽고 쓸 수 있다. 특정 사용자의 모든 저장소 설정에 적용된다.

.git/config : 이 파일은 Git 디렉토리에 있고 특정 저장소(혹은 현재 작업 중인 프로젝트)에만 적용된다. --local 옵션을 사용하면 이 파일을 사용하도록 지정할 수 있다. 하지만 기본적으로 이 옵션이 적용되어 있다. (당연히, 이 옵션을 적용하려면 Git 저장소인 디렉토리로 이동 한 후 적용할 수 있다.)
</p>

<h2>설정 확인</h2>
<pre>
$ git config --list
/
$ git config user.name
</pre>
<h2>.gitconfig 내용이 어디에서 설정되었는지 확인</h2>
<pre>
$ git config --show-origin rerere.autoUpdate
</pre>
<h2>도움말</h2>
<pre>git help ~~</pre>

<h1>Git 배우기 2</h1>
<h2>1. 버전관리 아직 안하는 로컬 디렉토리를 git 저장소로 만들기</h2>
<pre>
//1. 해당 파일로 이동한다.

$ git init //2. 어떤 파일도 관리하지 않는 뼈대 파일 생성

$ git add *.c //3. 저장소에 파일을 추가
$ git add LICENSE
$ git commit -m 'initial project version' //4. 커밋 수행
</pre>
<h2>2. 어딘가에서 git 저장소를 clone</h2>
<pre>
$ git clone https://github.com/libgit2/libgit2 mylibgit
//위 주소로부터 클론해오되, mylibgit 이름으로 가져온다
</pre>
<p>
https:// git:// user@server:path/to/repo.git(SSH) 등 다양한 프로토콜 지원 
</p>
<h2>수정 및 저장소에 저장</h2>
<img src = "./git_img/lifecycle.png">
<h3>워킹 디렉토리 내 파일의 상태</h3>
<ul>
<li>Untracked : 관리대상 X. git은 모르는 파일(<-> Tracked. 이하 모두 tracked 파일들)</li>
<li>Unmodified : 변경 안됨</li>
<li>Modified : 변경됨</li>
<li>Staged : 커밋으로 저장소에 기록 예정</li>
</ul>

<h3>파일 상태 확인</h3>
<pre>
$ git status
</pre>
<h3>파일 추적</h3>
<pre>
$ git add README
</pre>
<h3>Modified -> Staged</h3>
<pre>
$ git add README
</pre>
<p>git add 는 파일을 추가할 때, modified 상태의 파일을 staged 로 옮길 때 사용된다.</p>
<h3>파일 상태 짧게 확인</h3>
<pre>
$ git status -s
</pre>
<h3>파일 무시하기 (.gitignore 파일)</h3>
<p>
아무것도 없는 라인이나, `#`로 시작하는 라인은 무시한다.

표준 Glob 패턴을 사용한다. 이는 프로젝트 전체에 적용된다.

슬래시(/)로 시작하면 하위 디렉토리에 적용되지(Recursivity) 않는다.

디렉토리는 슬래시(/)를 끝에 사용하는 것으로 표현한다.

느낌표(!)로 시작하는 패턴의 파일은 무시하지 않는다.

Glob 패턴은 정규표현식을 단순하게 만든 것으로 생각하면 되고 보통 쉘에서 많이 사용한다. 애스터리스크( * )는 문자가 하나도 없거나 하나 이상을 의미하고, [abc] 는 중괄호 안에 있는 문자 중 하나를 의미한다(그러니까 이 경우에는 a, b, c). 물음표(?)는 문자 하나를 말하고, [0-9] 처럼 중괄호 안의 캐릭터 사이에 하이픈(-)을 사용하면 그 캐릭터 사이에 있는 문자 하나를 말한다. 애스터리스크 2개를 사용하여 디렉토리 안의 디렉토리 까지 지정할 수 있다. a/*/z 패턴은 a/z, a/b/z, a/b/c/z 디렉토리에 사용할 수 있다.
</p>
<pre>
#확장자가 .a인 파일 무시
*.a
#윗 라인에서 확장자가 .a인 파일은 무시하게 했지만 lib.a는 무시하지 않음
!lib.a
#현재 디렉토리에 있는 TODO파일은 무시하고 subdir/TODO처럼 하위디렉토리에 있는 파일은 무시하지 않음
/TODO
#build/ 디렉토리에 있는 모든 파일은 무시
build/
#doc/notes.txt 파일은 무시하고 doc/server/arch.txt 파일은 무시하지 않음
doc/*.txt
#doc 디렉토리 아래의 모든 .pdf 파일을 무시
doc/**/*.pdf
</pre>
<h3>Staged / Unstaged 변경 내용 보기</h3>
<pre>
$ git diff // unstaged 상태 변경 내용 보기
$ git diff <span>(--staged|--cached)</span> // staged 상태 변경 내용 보기
</pre>
<h3>변경 사항 commit</h3>
<pre>
$ git commit
</pre>
<h3>Staging area 생략</h3>
<pre>
$ git commit <span>-a</span>
</pre>
<h3>파일 삭제</h3>
<pre>
$ git rm ~~
</pre>
<p>git 에서 파일을 제거하면 해당 파일은 staged 상태가 된다. 이후 commit 하면 파일은 삭제되고, 이 파일을 더이상 추적하지 않는다. 만약 이미 파일을 수정했거나 Staging Area에 추가했다면, -f 옵션을 줘서 강제로 삭제해야 한다.
</p>
<h3>staging area에서만 삭제</h3>
<p>.gitignore 파일에 추가하는 것을 빼먹거나, 로그 파일, 컴파일된 파일 등을 실수로 추가했을 때 사용한다.</p>
<pre>
$ git <span>rm --cached</span> README
/
$ git rm log/<span>\</span>*.log //여러개의 파일을 동시에 제거
</pre>

<h3>파일 이름 변경</h3>
<p>git 자체는 파일 이름의 변경 혹은 파일의 이동을 명시적으로 관리하지 않고, 파일 이름의 변경 역시 별도의 정보를 저장하지 않는다. 진작에 파일 이름이 바뀌어도 해당 파일을 알아낼 수 있다. 단, 이름 변경 자체는 가능하다.</p>
<pre>
$ git mv README.md README
// 다음 코드를 축약한 것과 같다. 즉 단축된 표현일 뿐이다.
$ mv README.md README
$ git rm README.md
$ git add README
</pre>
<h2>커밋 히스토리 조회</h2>
<pre>
$ git log
</pre>
<h3>추가 옵션</h3>

<ul>
<li>-p, --patch : 각 커밋의 diff 결과를 보여줌(diff 결과와 동일)</li>
<li>-2 : 최근 2개의 결과만 보여줌</li>
<li>--stat : 각 커밋의 통계정보를 조회(파일에 대한 수정/삭제/변경 등의 내용)</li>
<li>--pretty=() : 히스토리 내용을 보여줄 때 여러 옵션 중 하나를 선택</li>
    <ul>
    <li>oneline</li>
    <li>short</li>
    <li>full</li>
    <li>fuller</li>
    <li>format : 커스텀 개념</li>
    <table>
    <thead>
    <tr>
    <th>옵션</th>
    <th>설명</th>
    </tr>
    </thead>
    <tbody>
    <tr>
    <td>%H</td>
    <td>커밋 해시</td>
    </tr>
    <tr>
    <td>%h</td>
    <td>짧은 길이 커밋 해시</td>
    </tr>
    <tr>
    <td>%T</td>
    <td>트리 해시</td>
    </tr>
    <tr>
    <td>%t</td>
    <td>짧은 길이 트리 해시</td>
    </tr>
    <tr>
    <td>%P</td>
    <td>부모 해시</td>
    </tr>
    <tr>
    <td>%p</td><td>짧은 길이 부모 해시</td>
    </tr>
    <tr>
    <td>%an</td>
    <td>저자 이름</td>
    </tr>
    <tr>
    <td>%ae</td>
    <td>저자 메일</td>
    </tr>
    <tr>
    <td>%ad</td>
    <td>저자 시각(형식은 –-date=옵션 참고)</td>
    </tr>
    <tr>
    <td>%ar</td>
    <td>저자 상대적 시간</td>
    </tr>
    <tr>
    <td>%cn</td>
    <td>커미터 이름</td>
    </tr>
    <tr>
    <td>%ce</td>
    <td>커미터 메일</td>
    </tr>
    <tr>
    <td>%cd</td>
    <td>커미터 시각</td>
    </tr>
    <tr>
    <td>%cr</td>
    <td>커미터 상대적 시각</td>
    </tr>
    <tr>
    <td>%s</td>
    <td>요약</td>
    </tr>
    </tbody>
    </table>
    </ul>
    <p>저자(Author)는 원 작업을 수행한 원작자, 커미터(Committer)은 마지막으로 이 작업을 적용한(저장소에 포함한) 사람이다. </p>
<li>--graph : 브랜치 / 머지 히스토리 보여주는 아스키 그래프 출력</li>
<li>--no-merges : 머지 커밋을 표시하지 않음</li>
</ul>
<h3>조회에 대한 제한 조건</h3>
<table>
<thead>
<tr>
<th>옵션</th>
<th>설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>-(n)</td>
<td>최근 n 개의 커밋만 조회한다.</td>
</tr>
<tr>
<td>--since, --after</td>
<td>명시한 날짜 이후의 커밋만 검색한다.</td>
</tr>
<tr>
<td>--until, --before</td>
<td>명시한 날짜 이전의 커밋만 조회한다.</td>
</tr>
<tr>
<td >--author</td>
<td>입력한 저자의 커밋만 보여준다.</td>
</tr>
<tr>
<td>--committer</td>
<td>입력한 커미터의 커밋만 보여준다.</td>
</tr>
<tr>
<td>--grep</td>
<td>커밋 메시지 안의 텍스트를 검색한다.</td>
</tr>
<tr>
<td>-S</td>
<td>커밋 변경(추가/삭제) 내용 안의 텍스트를 검색한다.</td>
</tr>
</tbody>
</table>
<pre>
$ git log --pretty="%h - %s" --author=gitster --since="2008-10-01" \ --before="2008-11-01" --no-merges -- t/
//특정 format으로 출력하되, author=gitster인 commit 중 2008-10-01~2008-11-01 인 순수 커밋만 확인
</pre>
<h2>커밋 되돌리기</h2>
<pre>
$ git commit <span>--amend</span>
</pre>
<p>Staging Area를 사용하여 커밋. 마지막 커밋 이후 수정 내용 없다면, 조금 전 커밋과 모든 것이 같다. 이옵션을 사용하여 커밋을 고치는 경우, 이전의 커밋은 없는 커밋이 되므로, 히스토리 상에 남지 않는다. 보통 사소한 작업(실수로 파일 하나 빼먹은 경우 등)에 대해 커밋을 추가로 만들지 않기 위해 이렇게 사용한다.</p>
<h3>파일 상태를 Unstage로 되돌리기</h3>
<pre>
$git reset HEAD [file]…
ex)
$git reset HEAD CONTRIBUTING.md
</pre>
<p>옵션을 주고 reset 사용시, 매우 위험할 수 있다.</p>
<h3>Modified로 되돌리기</h3>
<pre>
$ git checkout -- CONTRIBUTING.md
</pre>
<p>checkout 사용시, 변경된 내용을 모두 지우고 원 파일로 덮어쓴다. 또한 이 내용은 돌이킬 수 없음에 주의하자.</p>
<h2>리모트 저장소</h2>
<p>인터넷이나 네트워크 어딘가에 있는 저장소. 저장소의 특성에 따라 push/pull 가 다를 수 있다. 이름은 원격 저장소이지만, 반드시 외부에 있을 필요는 없으며, push/pull 여부가 더 중요한 점이다.</p>
<h3>리모트 저장소 확인</h3>
<pre>
$ git remote -v
</pre>
<p>저장소를 clone하면 'origin'이라는 리모트 저장소가 자동으로 등록된다. 또한 다수의 사람들과 작업하는 저장소의 경우, 여러개의 경로를 얻을 수 있다.
</p>
<h3>리모트 저장소 추가</h3>
<pre>
$ git remote add [단축이름] [url]
ex)
$ git remote add pd https://github.com/paulboone/ticgit
// https://github.com/paulboone/ticgit 을 pd로 리모트 저장소에 등록 
</pre>
<h3>Pull & Fetch</h3>
<h4>저장소에서 데이터 가져오기</h4>
<pre>
$ git fetch [remote]
</pre>
<p>로컬에는 없지만, 리모트 저장소에는 있는 데이터를 모두 가져온다. 이후, 리모트 저장소의 모든 브랜치를 로컬에서 접근할 수 있으므로, 언제든지 Merge & 내용을 볼 수 있다. 단, 수정된 내용을 모두 가져오기는 하나, 자동으로 Merge 하지 않으므로, 수동으로 Merge 해야 한다. git pull 의 경우, 자동으로 데이터를 가져오고, 로컬 branch와 merge 한다. git clone 명령은 자동으로 로컬의 master 브랜치가 리모트 저장소의 master 브랜치를 추적하도록 한다(물론 리모트 저장소에 master 브랜치가 있다는 가정에서). 그리고 git pull 명령은 Clone 한 서버에서 데이터를 가져오고 그 데이터를 자동으로 현재 작업하는 코드와 Merge 시킨다.
</p>
<h3>리모트에 Push</h3>
<pre>
$ git push [remote_name] [branch_name]
ex)
$ git push origin master // origin 리모트 저장소의 master 브랜치에 프로젝트 공유
</pre>
<p>Clone한 리모트 저장소에 쓰기 권한이 있고, Clone 이후 아무도 Upstream에 Push 하지 않았을 때만 사용이 가능하다. Clone한 사람이 여러명 있을때, 다른 사람이 Push 한 후에 Push 하려고 하면 불가능하다. 만약 Push하고 싶다면, 먼저 타인의 작업을 Merge 해와서 Push 해야 한다.</p>
<h3>리모트 저장소 살펴보기</h3>
<pre>
$ git remote show origin
</pre>
<ul>
<li>-v : 단축이름 + url. 등록된 모두를 보여준다</li>
</ul>
<h3>리모트 저장소 이름 바꾸기</h3>
<pre>
$ git remote rename pb paul
</pre>
<h3>리모트 저장소 삭제</h3>
<pre>
$ git remote remove paul
</pre>
<h2>Git tag</h2>
<p>주로 릴리즈할 때 태그를 사용한다</p>
<h3>태그 조회</h3>
<pre>
$ git tag [-l|--list] "v1.8.5*" // 태그 조회 [리스트 형태로] "패턴에 따라 조회"
</pre>
<p>와일드 카드를 이용한 패턴 사용시, 반드시 -l 옵션 지정.</p>
<h3>태그 붙이기</h3>
<ul>
<li>Lightweight : 브랜치와 유사하나, 최신 커밋으로 이동 X. 특정 커밋에 대한 포인터 역할 수행</li>
<li>Annotated : Git 데이터베이스 상에 개인 정보 및 날짜, 메시지도 저장하며, GPG서명도 가능하다. 특정 정보를 유지해야 하는 경우 사용하는 태그.</li>
</ul>
<h3>Annotated tag</h3>
<pre>git tag <span>-a</span> v1.4 -m "version 1.4"</pre>
<p>-a(Annotated), -s, -m(메시지) 등 옵션 추가.</p>
<pre>git show v1.4</pre>
<p>태그 정보 및 커밋 정보 확인. 태그를 만든 사람, 만든 시각, 메시지 등을 보여준다</p>
<h3>Lightweight tag</h3>
<pre>
$ git tag v1.4-lw
</pre>
<p>기본적으로 파일에 커밋 체크섬을 저장하는 것으로, 별다른 정보는 저장하지 않는다.</p>
<h3>나중에 태그 붙이기</h3>
<pre>
$ git tag [-a] name commit_checkSum
ex)
$ git tag -a v1.3 3fe11f7af -m "Additional tag" 
</pre>
<h3>태그 공유하기</h3>
<p>git push 명령은 자동으로 리모트 서버에 태그를 전송하지 않으므로, 별도로 Push 해야 한다. 이는 브랜치를 공유하는 것과 같은 방법으로 수행한다.</p>
<pre>
$ git push [origin] [v1.5/--tags]
</pre>
<h3>태그 Checkout</h3>
<pre>
$ git checkout [tag]
</pre>
<p>태그를 체크아웃하면 detached HEAD(떨어져나온 HEAD) 상태가 되며, 일부 git 관련 작업이 브랜치 작업과 다르게 동작할 수 있다. 이 상황에서 커밋을 만들면, 태그는 그대로 - 커밋은 새롭게 하나 쌓인 상태가 되고, 새 커밋에 도달할 방법은 따로 없게 된다(hash value 알면 가능하기는 하다). 따라서 버그 픽스처럼 의미 있는 행위를 원한다면
, 브랜치를 만들어 작업해야 한다.</p>
<pre>
$ git checkout -b version2 v2.0.0
</pre>
<p>이렇게 브랜치를 만들어 version2 브랜치에 커밋하면 브랜치는 업데이트 되나, v2.0.0 태그가 가리키는 커밋은 변하지 않으므로, 두 내용이 가리키는 커밋은 다르게 된다.</p>
<h2>git Alias</h2>
<p>기존에 많이 사용되는 명령어를 축약해서 사용하는 것</p>
<pre>
$ git config [--global] alias.co checkout // git co 로 git checkout 사용 가능
// 다른 예시들
$ git config --global alias.br branch
$ git config --global alias.ci commit
$ git config --global alias.st status
$ git config --global alias.visual '<span>!</span>gitk' // 외부 명령어도 가능
</pre>

<h2>Git Branch</h2>
<p>코드를 통째로 복사하고 나서 원 코드와는 상관 없이 독립적으로 개발을 진행하는 것. </p>
<p>Git은 데이터를 일련의 스냅샷으로 저장한다. 커밋을 하게 되면, 현재 Staging Area에 있는 데이터의 스냅샷에 대한 포인터, 저자, 커밋 메시지 등 메타데이터와 이전 커밋에 대한 포인터 등을 포함하는 Commit Object를 저장한다. 이전 커밋 포인터가 존재해 현재 커밋이 무엇을 기준으로 바뀌었는지 알 수 있는데, Merge 커밋의 경우 커밋 포인터가 여러개 존재한다.</p>
<pre>
$ git add README test.rb LICENSE
$ git commit -m 'The initial commit of my project'
</pre>
<p>
파일 수정 후 다시 커밋하면 이전 커밋의 정보도 저장한다. Git의 브랜치는 커밋 사이를 가볍게 이동할 수 있는 포인터 같은 것으로, 기본적으로 master 브랜치를 만든다. 처음 커밋시 master 브랜치가 생성된 커밋을 가리킨다. 이후 커밋을 만들면 master 브랜치는 가장 마지막 커밋을 가리킨다. 크게 특별한 점이 없으며, 그냥 초기 이름이고 변경하지 않는 것이다.
</p>
<h3>새 브랜치의 생성</h3>
<pre>
$ git <span>branch</span> testing
</pre>
<p>
새로 만든 브랜치는 현재 작업중인 마지막 커밋을 가리킨다.
</p>
<p>
Git은 HEAD라는 특수 포인터를 사용해 현재 작업중인 로컬 브랜치를 알 수 있다. git branch 은 브랜치를 만들기만 할 뿐, 작업중인 브랜치를 옮기지 않는다.
</p>
<pre>
$ git log --oneline <span>--decorate</span> //현재 실행중인 커밋 알기
$ git log --oneline --decorate --graph --all //갈라져 나온 브랜치 관련 히스토리 출력
</pre>
<h3>브랜치 이동</h3>
<pre>
$ git checkout testing // HEAD 포인터가 testing 을 가리킨다
</pre>
<p>
checkout을 통해 브랜치를 이동하면 워킹 디렉토리 역시 해당 시점의 파일로 돌려놓는다. 이후 커밋 하면 다른 브랜치 작업들과 별개로 진행되므로, 브랜치를 오가며 작업할 수 있다.
</p>
<ul>
<li>git branch : 새로운 브랜치 생성. 생성과 이동은 별개이며, 자동으로 이동 X</li>
<li>git checkout : 해당 브랜치로 이동</li>
<li>git commit : 커밋 작업</li>
</ul>
<h2>Branch & Merge</h2>
<p>중요한 문제 혹은 이슈가 발생하여 이를 처리할 필요가 있을 때 브랜치를 만들어 처리할 수 있다.
<ol>
<li>새로운 이슈를 처리하기 이전의 운영(Production) 브랜치로 이동한다.</li>
<li>Hotfix 브랜치를 새로 하나 생성한다.</li>
<li>수정한 Hotfix 테스트를 마치고 운영 브랜치로 Merge 한다.</li>
<li>다시 작업하던 브랜치로 옮겨가서 하던 일 진행한다.</li>
</ol>
</p>
<h3>브랜치 만들면서 이동</h3>
<pre>
$ git checkout <span>-b</span> [branch]
</pre>
<h3></h3>
<pre>
$ git merge [target] // 현재 브랜치와 타겟을 합친다
</pre>