# flatMap vs flatMapMany

## flapMap

### 조건별 flapMap

- Mono 가 empty 가 아닐때 error throw
  
  > Mono.<E> error
  > 형식으로 리턴

```
public Mono<UserChannel> createUserChannel(UserChannel userChannel){
        return getUserChannelByAuthKeyAndChannelType(userChannel.getChannelType(), userChannel.getAuthKey()) // channelType, authKey 중복 체크
                .flatMap(channel -> 
                        Mono.<UserChannel>error(new ServerErrorException(ServiceErrorMessage.ERR_AUTH_KEY_DUPLICATED.getMessage())))
                .switchIfEmpty(Mono.defer(() -> 
                        userChannelRepository.getUserChannelByUidAndChannelType(userChannel.getUid(), userChannel.getChannelType()))) // uid, channelType 중복 체크
                .flatMap(channel -> 
                        Mono.<UserChannel>error(new DuplicateKeyException("uid=" + channel.getUid() + ", channelType=" + channel.getChannelType())))
                .switchIfEmpty(Mono.defer(() -> 
                        userChannelRepository.save(userChannel)))
                ;


    }
```

> Mono.<E> error 를 사용하지않고 Mono.error 을 사용하면 <br>
> `Mono<Object>` 타입으로 리턴이 되어 flatMap 에서 원하는 타입을 사용이 불가능하다.

[Mono is not empty 일때 error throw ](https://stackoverflow.com/questions/58127132/how-to-perform-an-action-only-if-the-mono-is-empty-and-throw-an-error-if-not-emp)
