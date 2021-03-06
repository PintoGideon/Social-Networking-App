# Dev Social, A Social Networking App

# Building blocks for each components


![new doc 2019-04-06 23 57 41-1](https://user-images.githubusercontent.com/15992276/55678466-4375ae00-58c8-11e9-992e-3da9203104f8.jpg)

 
# Data Model for the User

Each user should have a name, email, password, avatar, data

  ```javascript

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

```javascript

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


Register

The Form creates a new user data and fires an action.

    ```javascript
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

```javascript
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


```javascript

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

```javascript

const initialState = {
isAuthenticated: false,
user: {}
};

```
# In the Reducer

```javascript
switch (action.type) {
case SET_CURRENT_USER:
return {
...state,
isAuthenticated: !isEmpty(action.payload),
user: action.payload
};

```


Now the auth state can be used throughout the application to see if the user has logged in for Private Routes.


