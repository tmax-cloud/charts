1. redirect 검증해주기 위해서 whitelist에 hyperauth 주소를 넣어줘야함 
   - 안넣어주면 로그아웃이 제대로 되지않음 
2. redis db로 토큰저장하여 사용하기 위해선 cookie-name에 . 이 들어가면 안됨 
   - https://github.com/oauth2-proxy/oauth2-proxy/issues/978

3. redis db 값 확인 
   - 비밀번호 확인 
     - secret 으로 저장되어 있음 
     - .data.redis-password | base64 -d
   - master-0 파드에 접속 후 redis-cli 입력 
   ```shell
   redis-cli
   ```
   - AUTH 명령어를 통해 로그인 
   ```shell
   AUTH $PASSWORD
   ```
   [예시 링크](https://yongku.tistory.com/entry/Redis-redis-cli%EC%97%90%EC%84%9C-%EB%B9%84%EB%B0%80%EB%B2%88%ED%98%B8-%EC%84%A4%EC%A0%95%ED%95%98%EB%8A%94-%EB%B0%A9%EB%B2%95)
   [redis 기본 명령어](https://littleshark.tistory.com/69)

4. cors 명령어 이해 -> 문제 해결 
할일: traefik 에 cors 적용 내용 정리
할일: oauth2proxy에 cors 적용 내용 정리 
5. [cors쿠키전송하기](https://inpa.tistory.com/entry/AXIOS-📚-CORS-쿠키-전송withCredentials-옵션)
5. [다른도메인간쿠키전송](https://velog.io/@wiostz98kr/다른-도메인간-쿠키-전송하기)
5. [cors 정리된 블로그](https://evan-moon.github.io/2020/05/21/about-cors/)
6. [js-fetch에 credential 적용](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch)

[oauth2proxy적용설명](https://developer.okta.com/blog/2022/07/14/add-auth-to-any-app-with-oauth2-proxy)