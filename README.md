# Dev Social, A Social Networking App

# Building blocks for each components

![flow-1](https://user-images.githubusercontent.com/15992276/55678013-73b94e80-58c0-11e9-8ced-d4e750e9ab10.JPG)
![flow-2](https://user-images.githubusercontent.com/15992276/55678015-76b43f00-58c0-11e9-8a16-fbd2f795f872.JPG)
 
# Data Model for the User

Each user should have a name, email, password, avatar, data

  ```

const UserSchema = new Schema({
name: {
type: String,
required: true
},
email: {
type: String,
required: true
},
password: {
type: String,
required: true
},
avatar: {
type: String
},
date: {
type: Date,
default: Date.now
}
});

```

# Creating routes for Login and Register Components

Routes

```

# In server.js

 app.use('/api/users',users)

# In users.js

router.post('/register', (req, res) => {
{
return res.status(400).json(errors);
}

router.post('/login', (req, res) => {
const { errors, isValid } = validateLoginInput(req.body);

    return res.status(400).json(errors);

}

````

# Authentication

A Passport strategy for authenticating with a JSON Web Token.
This module lets you authenticate endpoints using a JSON web token. It is intended to be used to secure RESTful endpoints without sessions.

Reference Link: http://www.passportjs.org/packages/passport-jwt/


# Login and Register Components

![Register Component](https://user-images.githubusercontent.com/15992276/55678022-9ea3a280-58c0-11e9-9415-8bc8452b907e.JPG)
![LOGIN COMPONENT](https://user-images.githubusercontent.com/15992276/55678021-9ea3a280-58c0-11e9-9ae9-091cb4b9e3fa.JPG)


Register

The Form creates a new user data and fires an action.

    ```
    onSubmit = e => {
		e.preventDefault();

		const newUser = {
			name: this.state.name,
			email: this.state.email,
			password: this.state.password,
			password2: this.state.password2
		};

		this.props.registerUser(newUser, this.props.history);
	};

    ```

The action sends a request to the API and redirects the    user to the login page if the post request is successful.

```
    axios
		.post('/api/users/register', userData)
		.then(res => history.push('/login'))




    Login


    onSubmit = e => {
    	e.preventDefault();

    	const userData = {
    		email: this.state.email,
    		password: this.state.password
    	};

    	this.props.loginUser(userData);
    };

```

# Sends the data to the login action. Extracts the data out of the token which was created using Passport JWT strategy.


```

axios
.post('/api/users/login', userData)
.then(res => {

# Save to localStorage

const { token } = res.data;

# Set token to localStorage

localStorage.setItem('jwtToken', token);

# Set token to Auth header

setAuthToken(token);

# Decode token to get user data

const decoded = jwt_decode(token);

# Set current user

dispatch(setCurrentUser(decoded));
})

```


The action setCurrentUser sets the newState via the reducer to which the action is dispatched in Redux.

```

const initialState = {
isAuthenticated: false,
user: {}
};

```
# In the Reducer

```
switch (action.type) {
case SET_CURRENT_USER:
return {
...state,
isAuthenticated: !isEmpty(action.payload),
user: action.payload
};

```


Now the auth state can be used throughout the application to see if the user has logged in for Private Routes.


