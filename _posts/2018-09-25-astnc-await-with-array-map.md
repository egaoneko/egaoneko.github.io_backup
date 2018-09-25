---
layout: post
title: 'Array.map에서 async, await 사용'
date: 2018-09-25 12:30:00
categories: javascript
tags: [javascript, til]
published: true
fullview: false
comments: true
---

`async` 와 `await`을 사용하는 경우에, 이미 조회한 리스트의 각 요소에 대해 추가로 조회하여 반환해야하는 경우에 대해 어떻게 처리해야할까 찾아보았다.

stack overflow에서 찾아보니 아래와 같이 `Promise.all`과 `Array.map`을 사용하여 처리하는 것을 보았다.

```javascript
const arr = [1, 2, 3, 4, 5];

const results: number[] = await Promise.all(arr.map(async (item): Promise<number> => {
    await callAsynchronousOperation(item);
    return item + 1;
}));
```

동일한 방법으로 사용자의 목록을 불러온 뒤에 개별 사용자의 사진을 불러올 때 사용하니 의도한대로 동작하였다.

```javascript
  @get('/users')
  async find(
    @param.query.string('filter') filter: Filter,
  ): Promise<UserInterface[]> {
    const users: User[] = await this.userRepository.find(filter);
    return await Promise.all(
      users.map(
        async (user: User): Promise<UserInterface> => {
          const picture: Picture = await this.pitureRepository.findById(
            user.pictureId,
          );
          return {
            id: user.id,
            email: user.email,
            name: user.name,
            picture: picture,
            role: user.role,
          } as UserInterface;
        },
      ),
    );
  }
```

* 참고
  * [Use async await with Array.map - stack overflow](https://stackoverflow.com/questions/40140149/use-async-await-with-array-map)
  * [nibble repo](https://github.com/egaoneko/nibble/blob/0d776890d98b80e3bac1a1b3d869a5039831238e/server/src/controllers/user.controller.ts#L74)