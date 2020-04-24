4월 22, 2020  2020.04.22

[https://www.udemy.com/course/redux-saga/learn/lecture/12335832#overview](https://www.udemy.com/course/redux-saga/learn/lecture/12335832#overview)

# 큰그림 복습하고 시작

[1] Provider 를 통해 redux store 를 App component 에 주입. (즉, 어플리케이션 전체에 적용된다.)

[2] App component 내에서 connect 함수를 통해 action 이 발생하면 reducer 를 찾아가도록 연결해준다.

[3] 원래 redux 작동방식이라면 1,2에서 끝났을텐데... redux-saga 라는 middleware 를 통해 action이 reducer 를 찾아가기 전에 중개를 해준다!

### axios.default.withCredentials = true

XMLHttpRequest from a different domain cannot set cookie values for their own domain unless withCredentials is set to true before making the request.

    axios.get('some api url', {withCredentials: true});

If you're having issues with REM REST API or if the server is down for some reason, replace your axios default code with this:

    // 이 경우, withCredentials true 설정 안함.
    // axios.defaults.withCredentials = true;
    axios.defaults.baseURL = 'https://cors-anywhere.herokuapp.com/https://rem.dbwebb.se/api';

A good rule of thumb is to align your saga's structure similar to the structure of your redux actions.

### takeEvery

There's a number of ways we can design the way saga's react to dispatched actions.

The easiest helper to understand is the takeEvery helper which as we've seen in the lecture on a practical

look at saga's is used to take every dispatched action that we specify.

You may be thinking

"Well isn't that true for any action we dispatch?"

"Surely we'd want to apply takeEvery to, well, every action that's dispatched."

⇒action 이 들어왔다면(보내졌다면, dispatch 되었다면) 다 받아서 처리하는게 맞지 않나??

### 위의 의문에 대한 대답 (예시)

So consider when we delete a user. If the call to delete a specific user via the API hasn't resolved

yet.

So if this called the API hasn't resolved yet.

But then the user hits the delete button again.

Triggering this saga here because we're watching the delete user request.

So if this is still resolving.

We're calling delete user here which is this.

Generator function here.

If this is still resolving while the user then triggers another delete user request.

⇒ 그니까, 유저를 추가하거나 지우는 (뭐 게시글이 될 수도 있겠지) 요청이 아직 끝나지 않았는데, 사용자가 요청을 중복으로 보낼 경우, 해당 요청을 막고 이전에 들어온 요청이 먼저 해결되도록 한다는 의미!!

⇒blocking saga (밑에 deleting user 에서 등장)

Obviously we can do this and we should be doing this in the UI as well.

You know we we should be disabling the delete buttons when a user is being deleted.

But this is also the whole point of saga's is to describe the flow of how side effects are called.

⇒이런 맥락에서 takeEvery 는 non-blocking saga. (무조건 다 받으니까)

### watcher saga

So then we want to actually write the watcher saga which will watch for every time the get users request

action is dispatched.

    function* watchGetUsersRequest(){
    	yield takeEvery(actions.Types.GET_USERS_REQUEST, getUsers);
    }

### worker saga

So this get users which we're going to right now is referring to the worker saga.

    function* getUsers(){
    	try{
    		const result = yield call(api.getUsers);
    		yield put(actions.getUsersSuccess({
    			items: result.data.data
    		}));
    	}catch(e){
            yield put(actions.usersError({
                error: 'An error occurred when trying to get the users'
            }));
    	}
    }

# JS Generator functions

You don't call a generator function and expect each line to run and then finish in one process.

So generator functions must always yield values when we hit the yield keyword in a generator function

it returns a value but then it waits for us to instruct it to run again.

(yield 명령어가 작동할 때마다 값을 하나씩 뱉어낸다.)

So we can have one yield or multiple yield statements in a generator function but we must call it in

such a way that iterates through each yield until there's no more code left to run the generator in

which the generator then terminates.

### redux saga 에서 generator function

So in the context of redux saga.

In the vast majority of cases we never actually terminate to the generator function.

So again in the context of redux saga under the hood of the library this is exactly how

redux saga watcher sagas are constantly watching for redux actions that were dispatched.

They're actually all running within these wild true loops.

So they're all running in a while true loop.

### takeEvery 의 작동방식

So again in the context of redux saga under the hood of the library this is exactly how

redux saga watcher sagas are constantly watching for redux actions that were dispatched.

They're actually all running within these wild true loops.

So they're all running in a while true loop.

### call (effects)

Now what call does is it allows us to call a promise for example and call it sequentially so we're just

waiting for it to resolve.

So we're not actually going to be writing any call-backs or anything like that to it's best to see it

as an example.

    const result = yield call(api.getUsers);
    		yield put(actions.getUsersSuccess({
    			items: result.data.data
    		}));

Now what this is going to do is once this call to api.getUsers resolves it's going to assign the

result to this const result.

Any code here afterwards will be run once this call has resolved.

### fork(effects)

⇒일단, c 로 process fork 하던거랑 같은 의미임 ㅇㅇ

We're going to be introduced to another effect here and it's called fork.

If you're already familiar or have coded in languages before where process creation is commonplace then

the concept is exactly the same here in redux saga.

### 근데 여기서 fork , 즉 프로세스 분리가 필요한 이유가 뭔데??

Well if you think about it if we've got separate processes running all our watches all our logic is

nicely separated into these separate processes.

(1) 하나의 작업에서 발생하는 오류가 다른 작업에 영향을 끼치지 않도록

So any errors that occur here we can catch effectively and act upon without affecting these set these

second and third processes.

(2) 병렬적 작업 처리.

Also we can run these in parallel so we're not waiting for get to users.

The watch gets users request saga to run before then running the delete users request saga.

For example. We're running them all in parallel.

### all (effects)

⇒Promise 전부 다 resolve 하는 역할.

For example. If you've used promises in javascript and you've got multiple promises and you want to resolve

them all at the same time and then only act upon it.

Once all those are resolved.

That's essentially what yield all does in read saga.

Except we're doing it with the forked processes.

    export default function* rootSaga(){
    	yield all([
    		...userSagas
    	]);
    }

⇒ So what this yield all will do in the root saga is allow all these forked processes to be created in parallel.

(We're firing off all those promises that we pass it to and running them in parallel and then waiting for all of them to resolve.)

### spread operator (...)

⇒ 이거 이름이 spread operator 구나!!

So we want to use the spread operator here so all the spread operator does if you haven't come across it

before is create a new array from this uses saga's array that we've imported to it copies it into this

array we've created here as part of yield all.

---

---

### 전체적인 흐름을 한 번 다시 정리해보자.

[1] app component 에서 getUsersRequest 발생

[2] watcher saga (= watchGetUsersRequest )에서 요청 들어온거 파악

[3] worker saga (= getUsers) 발동

[4] getUsers 안에 axios 를 이용하여 api 호출하는 코드 있음.

    const result = yield call(api.getUsers); // 이렇게!

[5] 호출이 성공하면 getUsersSuccess 라는 action 이 발동되고, 실패하면 usersError action 이 발동됨 (dispatch 된다)

### put (effects)

    yield put(actions.getUsersSuccess({
    			items: result.data.data
    		}));
    // put effect 가 dispatch 로 쓰인다!

[6] getUsersSuccess 혹은 usersError action 이 users reducer 를 향해 오게 되어있기 때문에

[7] users reducer 에서 action type 을 분기하여 state managing 작업 시작.

### takeLatest (effect)

takeLatest helper works in a very similar way to the take every helper every time.

The action we specify to take latest is dispatched the saga acts upon it.

[ex]

For argument's sake let's say it takes 10 seconds for the entire create user saga and the API call to resolve.

If we're say five seconds into that but we hit the create user button again if we're using Take latest. Then that first saga call is actually cancelled assuming it's still running.

(이미 진행되고 있던 요청이 이미 있었다면, 그 요청을 제거하고 새로 들어온 요청을 처리하도록 한다는 의미!)

⇒ 집구하기 에서도 만약 세부사항 입력이 많아지거나 하나의 요청에 많은 시간이 소요된다면, takeEvery 대신 takeLatest 사용할 수 있지 않을까??

[활용 예시]

The reason I like using Take latest for things like creating or updating records is if the user enters

their details the call is resolving but they realize there's a typo or something in the data they've

entered. So they quickly change the data and submit the form again.

⇒ 오호... 입력할 세부사항이 많은 form 을 submit 하는 경우에 유용하게 사용 가능할듯!

### call effect 를 사용하는데(api call) 여러가지 param 을 담고 싶을 때

    yield call(api.createUser, {
                firstName: payload.firstName,
                lastName: payload.lastName
            });
    // 이런 식으로 두 번째 인자에 {} 이렇게 넣어주세요. 하나만 있으면 그냥 그거 넣으면 되고 ㅇㅇ
    yield call(api.deleteUser, userId); <= 하나만 있을 때
    

## take (effect)

    function* watchDeleteUserRequest(){
        while(true){
            const {payload} = yield take(actions.Types.DELETE_USER_REQUEST);
            yield call(deleteUser, payload.userId);
        }
    }

We can't actually pass in a worker saga to the '**take**' effect because it's a lower level effect.

Take is a lower level helper that simply returns the action that was dispatched.

take 는 일회성이라는 의미네. 그냥 'take(~~)' 에서, ~~ 자리에 들어온(dispatch 된) aciton 을 한 번 받는다는 의미! ( 위 코드에서는 해당 action 에 실린 payload 만 빼낸거지.)

takeEvery 나 takeLatest 는 사실 redux-saga library 에서 제공되는 helper 이다?!

So these are actually helpers takeLatest and takeEvery provided by the redux saga library.

### 이렇게 하는 이유??

But until any of this resolves we're actually unable to come back into this while loop.

즉, 

            yield call(deleteUser, payload.userId);

이 코드에서 실행되는 worker saga ( = deleteUser ) 에서 작업이 완료될 때까지 while loop 을 넘어가지 않게 된다. (하나의 유저를 다 지우고 나서야 다른 사용자를 지우는 작업이 수행된다.)

⇒ Essentially this watch delete user request is ignoring any additional delete user request actions that may have been dispatched.

### handlign error (showing the Error to the users)

    handleCloseAlert = () => {
            this.props.usersError({
                error: '' => 이렇게 error 내역을 초기화 해주어야 아래 isOpen 값이 false 로 되어 Alert 가 사라지게 된다.
            });
        };
    
    <Alert color="danger" isOpen={!!this.props.users.error} toggle={this.handleCloseAlert}>
                        {this.props.users.error}
                    </Alert>
    
    => reactstrap 을 활용한 구현 => material-ui 에도 분명 있을 걸로 생각됨 ㅇㅇ

# **Redux Saga Cheat Sheet**

Examples of when to use various Redux Saga keywords and techniques:

# **"takeEvery"**

- ***Use this when:*** You want to watch for EVERY time a specific redux action was dispatched.
- ***Use case:*** Getting / fetching a list of data from an API.
- ***Example:***

    function* watchGetUsersRequest(){ yield takeEvery(action.Types.GET_USERS_REQUEST, getUsers);}

# **"takeLatest"**

- ***Use this when:*** There's the potential for a redux action to be dispatched multiple times in a short period and could potentially initiate the running of multiple instances of the same saga - use takeLatest to ONLY take the latest currently running saga for the associated dispatched redux action.
- ***Use cases:*** Creating or updating a record, or;

    If you have a complex app that queries the same API endpoint from multiple components at the same time - for example if you have a navbar that displays the currently logged in user's name, but the user is viewing a 'settings' page to view their personal details meaning both the navbar and the settings page will query the same API endpoint - you'll generally want to take the latest call for that data.

- ***Example:***

    function* watchGetLoggedInUserRequest(){
    yield takeLatest(action.Types.GET_LOGGED_IN_USER_REQUEST, getLoggedInUser);
    }

# **Blocking saga with "take"**

- ***Use this when:*** You want to watch for a particular redux action to be dispatched, but you don't want to listen for that same dispatched action again until the currently running saga for that action has complete. You're "blocking" the ability to watch for when that particular redux action is dispatched until the currently running saga for that redux action has complete.
- ***Use case:*** Deleting a user, or;

    Accepting a payment. Generally you don't want to be able to accept multiple, simultaneous payments - you'd want to wait for the current transaction to complete before allowing the ability to accept another payment.

- ***Example:***

    function* watchDeleteUserRequest(){
    	while(true){
    		const {userId} = yield take(action.Types.DELETE_USER_REQUEST); yield call(deleteUser, {userId});
    	}
    }

# **"call"**

- ***Use this when:*** You want to call a function or a promise but want to wait for that function or promise to finish running before executing the next line of code.
- ***Use case:*** In conjunction with "take" and blocking saga, or;

    Calling a promise within a worker saga that queries an API endpoint.

- ***Examples:***

    function* deleteUser({userId}){
    	try{
    		const result = yield call(api.deleteUser, userId);
    	}
    	catch(e){  }
    }
    function* watchDeleteUserRequest(){
    	while(true){
    		const {userId} = yield take(action.Types.DELETE_USER_REQUEST);yield call(deleteUser, {userId});
    	}
    }

# **"put"**

- ***Use this when:*** You want to dispatch a redux action from within a redux saga.
- ***Use case:*** Any time you want to update your redux state - usually after a call to an API resolves and you want to update your redux state with the resulting data from the API.
- ***Examples:***

    function* getUsers(){
    	try{
    		const result = yield call(api.getUsers);
    		yield put(actions.getUsersSuccess({ users: result.data.users }));
    	}
    	catch(e){
    	}
    }
