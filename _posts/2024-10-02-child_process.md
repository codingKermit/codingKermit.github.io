---
_filters: 
title: node.js의 child_process 모듈
date: 2024-10-02
categories:
  - javascript
  - Node.js
tags:
  - Node
  - javascript
---
자식 프로세스를 생성하는 모듈입니다. `worder_threads` 와 비교하자면 `worker_threads` 는 하나의 프로세스가 다수의 스레드를 사용 하도록 합니다. `child_process` 는 새로운 자식 프로세스를 실행 시킵니다.  
새로운 프로세스를 실행시키기 때문에 아예 다른 언어를 실행 할 수도 있습니다.  
프로세스 생성에는 다음의 두 가지 방법이 있습니다.
1. [비동기 프로세스 생성](#비동기-프로세스-생성)
2. [동기 프로세스 생성](#동기-프로세스-생성)

단 자식 프로세스가 실행하고자 하는 프로그램은 설치가 되어있어야 합니다. 이 모듈이 프로그램을 대신 동작하는 것이 아닌, 해당 프로그램이 동작하도록 요청하는 작업을 하기 때문입니다.

### 비동기 프로세스 생성
비동기 프로세스는 부모 프로세스가 자식 프로세스의 종료를 기다리지 않고 다음 작업을 수행하는 프로세스 입니다.  
비동기 프로세스를 생성하는 방법에는 다음과 같은 방법이 있습니다.
1. [`exec(command[, options][, callback])`](#execcommand-options-callback)
2. [`execFile(file[, args][, options][, callback])`](#execfilefile-args-options-callback)
3. [`fork(modulePath[, args][, options])`](#forkmodulepath-args-options)
4. [`spawn(command[, args][, options])`](#spawncommand-args-options)

#### `exec(command[, options][, callback])`
쉘을 통해 명령어를 실행하고 그 결과를 버퍼(buffer)로 리턴받습니다.  
- `command` : 실행할 명령어
- `options` : 객체로써 전달되며 `child_process` 모듈에서 지정한 옵션을 지정할 수 있습니다
- `callback` : 프로세스 종료 후 호출될 함수

##### exec의 options 종류

| 옵션          | 타입                | 설명                                                                                      |
| ----------- | ----------------- | --------------------------------------------------------------------------------------- |
| cwd         | string            | 자식 프로세스의 작업 디렉토리                                                                        |
| env         | Object            | 환경변수. 키-값 의 형태. <br>기본값 : process.env                                                   |
| encoding    | string            | 인코딩. <br>기본값 : utf8                                                                     |
| shell       | string            | 명령을 실행할 셸.<br>기본값<br>- Unix 운영 체제 => `/bin/sh` <br>- 윈도우 운영 체제 => `process.env.ComSpec` |
| signal      | AbortSignal       | AbortSiganl을 사용하여 자식 프로세스 중단                                                            |
| timeout     | number            | 종료 시간. <br>기본값 : 0                                                                      |
| maxBuffer   | number            | `stdout` 혹은 `strerr` 에서 허용되는 최대 데이터 양(바이트 단위)<br>값을 초과하면 자식 프로세스가 종료되고 초과된 출력은 잘립니다.    |
| killSignal  | string \| integer | 자식 프로세스가 abort 혹은 timeout 등의 이유로 강제 종료 되었을 때 사용할 신호.<br>기본값 : `SIGTERM`                 |
| uid         | number            | 프로세스의 uid(사용자 ID) 설정                                                                    |
| gid         | number            | 프로세스의 gid (그룹 ID) 설정                                                                    |
| windowsHide | boolean           | 새로운 콘솔 창을 숨깁니다<br>기본값 : false                                                           |

#### `execFile(file[, args][, options][, callback])`
실행 파일을 실행 합니다. 쉘을 통하지 않습니다. 결과를 버퍼로 받습니다.  
- `file` : 실행할 파일의 이름이나 경로
- `args` : 문자열 인수 목록
- `options` : 객체로써 전달되며 `child_process` 모듈에서 지정한 옵션을 지정할 수 있습니다
- `callback` : 프로세스 종료 후 호출될 함수

##### execFile의 options 종류

| 옵션                       | 타입                | 설명                                                                                      |
| ------------------------ | ----------------- | --------------------------------------------------------------------------------------- |
| cwd                      | string            | 자식 프로세스의 작업 디렉토리                                                                        |
| env                      | Object            | 환경변수. 키-값 의 형태. <br>기본값 : process.env                                                   |
| encoding                 | string            | 인코딩. <br>기본값 : utf8                                                                     |
| timeout                  | timeout           | 종료 시간. <br>기본값 : 0                                                                      |
| maxBuffer                | number            | `stdout` 혹은 `strerr` 에서 허용되는 최대 데이터 양(바이트 단위)<br>값을 초과하면 자식 프로세스가 종료되고 초과된 출력은 잘립니다.    |
| killSignal               | string \| integer | 자식 프로세스가 abort 혹은 timeout 등의 이유로 강제 종료 되었을 때 사용할 신호.<br>기본값 : `SIGTERM`                 |
| uid                      | number            | 프로세스의 uid(사용자 ID) 설정                                                                    |
| gid                      | number            | 프로세스의 gid (그룹 ID) 설정                                                                    |
| windowsHide              | boolean           | 새로운 콘솔 창을 숨깁니다<br>기본값 : false                                                           |
| windowsVerbatimArguments | boolean           | 인수의 이스케이프 처리 여부<br>Unix 운영 체제에서는 무시됩니다.<br>기본값 : false                                  |
| shell                    | boolean \| string | 명령을 실행할 셸.<br>기본값<br>- Unix 운영 체제 => `/bin/sh` <br>- 윈도우 운영 체제 => `process.env.ComSpec` |
| signal                   | AbortSignal       | AbortSiganl을 사용하여 자식 프로세스 중단                                                            |

#### `fork(modulePath[, args][, options])`
쉘을 사용하지 않으며 결과를 스트림으로 리턴 받습니다.  
주요 특징은 다음과 같습니다.
- Node.js 전용으로 새로운 Node.js 프로세스를 생성
- 부모 프로세스와 자식 프로세스 간에 `message` 이벤트를 통한 통신 가능

##### fork의 options 종류

| 옵션                       | 타입                | 설명                                                                      |
| ------------------------ | ----------------- | ----------------------------------------------------------------------- |
| cwd                      | string            | 자식 프로세스의 작업 디렉토리                                                        |
| detached                 | boolean           | 자식 프로세스가 독립적으로 실행되도록 합니다.<br>기본값 : false                                |
| env                      | Object            | 환경변수. 키-값 의 형태. <br>기본값 : process.env                                   |
| execPath                 | string            | 자식 프로세스를 만드는데 사용되는 실행 파일                                                |
| execArgv                 | sring[]           | 실행 파일에 전달된 문자열 인수 목록<br>기본값 : process.execArgv                          |
| gid                      | number            | 프로세스의 gid (그룹 ID) 설정                                                    |
| serialization            | string            | 프로세스 간에 주고받는 메세지의 종류<br>기본값 : json                                      |
| signal                   | AbortSignal       | AbortSiganl을 사용하여 자식 프로세스 중단                                            |
| killSignal               | string \| integer | 자식 프로세스가 abort 혹은 timeout 등의 이유로 강제 종료 되었을 때 사용할 신호.<br>기본값 : `SIGTERM` |
| silent                   | boolean           | `stdout` , `stderr` 이 부모 프로세스와 입출력을 공유할지 여부<br>기본값 : false              |
| stdio                    | Array \| string   | 자식 프로세스와 부모 프로세스 간의 입출력 연결 방법                                           |
| uid                      | number            | 프로세스의 uid(사용자 ID) 설정                                                    |
| windowsVerbatimArguments | boolean           | 인수의 이스케이프 처리 여부<br>Unix 운영 체제에서는 무시됩니다.<br>기본값 : false                  |
| timeout                  | number            | 종료 시간. <br>기본값 : 0                                                      |

#### `spawn(command[, args][, options])`
스트림(stream) 데이터를 리턴 받습니다. 대규모 데이터를 처리하거나 실시간 출력을 모니터링 할 때 유리합니다.  

##### spawn의 options 종류

| 옵션                       | 타입                | 설명                                                                                      |
| ------------------------ | ----------------- | --------------------------------------------------------------------------------------- |
| cwd                      | string            | 자식 프로세스의 작업 디렉토리                                                                        |
| env                      | Object            | 환경변수. 키-값 의 형태. <br>기본값 : process.env                                                   |
| argv0                    | string            |                                                                                         |
| stdio                    | Array \| string   | 자식 프로세스와 부모 프로세스 간의 입출력 연결 방법                                                           |
| detached                 | boolean           | 자식 프로세스가 독립적으로 실행되도록 합니다.<br>기본값 : false                                                |
| uid                      | number            | 프로세스의 uid(사용자 ID) 설정                                                                    |
| gid                      | number            | 프로세스의 gid (그룹 ID) 설정                                                                    |
| serialization            | string            | 프로세스 간에 주고받는 메세지의 종류<br>기본값 : json                                                      |
| shell                    | boolean \| string | 명령을 실행할 셸.<br>기본값<br>- Unix 운영 체제 => `/bin/sh` <br>- 윈도우 운영 체제 => `process.env.ComSpec` |
| windowsVerbatimArguments | boolean           | 인수의 이스케이프 처리 여부<br>Unix 운영 체제에서는 무시됩니다.<br>기본값 : false                                  |
| windowsHide              | boolean           | 새로운 콘솔 창을 숨깁니다<br>기본값 : false                                                           |
| signal                   | AbortSignal       | AbortSiganl을 사용하여 자식 프로세스 중단                                                            |
| timeout                  | number            | 종료 시간. <br>기본값 : 0                                                                      |
| killSignal               | string \| integer | 자식 프로세스가 abort 혹은 timeout 등의 이유로 강제 종료 되었을 때 사용할 신호.<br>기본값 : `SIGTERM`                 |
 
### 동기 프로세스 생성
동기 프로세스는 자식 프로세스의 작업이 끝날 때 까지 부모 프로세스가 대기하는 프로세스 입니다.  
동기 프로세스 생성 방법은 다음과 같은 방법이 있습니다.  
1. `execFileSync(file[, args][, options])`
2. `execSync(command[, options])`
3. `spawnSync(command[, args][, options])`

각각의 함수는 Sync가 빠진 함수와 기능적으로 매칭되며 옵션이 같기 때문에 생략합니다.


---
참조 : [https://nodejs.org/docs/latest/api/child_process.html](https://nodejs.org/docs/latest/api/child_process.html)  