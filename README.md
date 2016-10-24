To test this component we'll need to import it into `App.js` and add the component inside of the div with the class of "friends". Because we haven't yet added an `onChange` handler none of these values are editable, so let's fix that!

* Begin by creating a new method on the `FriendsList` class named `handleChange`.
	* This method should take in two parameters, `field` and `event`.
	* When called, the method should use React's `setState` method to change the value of the correct property on state to `event.target.value`.
* Next we need to add `onChange` properties to our select and input elements, passing in our `handleChange` method.
	* Don't forget to use `bind` to preserve the context of `this`!
	* Make sure `onChange`'s `field` parameter matches the field on state that you want to change.


**Checkpoint:** You should now be able to make changes to your search field and select boxes and have that value placed on `FriendsList`'s `state`. Your code should look something like this:

``` jsx
import React from "react";

class FriendsList extends React.Component {
	constructor( props ) {
		super( props );

		this.state = {
			  searchText: ""
			, orderBy: "name"
			, order: "ascending"
		};
	}

	handleChange( field, event ) {
		this.setState( { [ field ]: event.target.value } );
	}

	render() {
		return (
			<div>
				<form
					className="form-inline searchForm"
					role="form"
				>
					<div className="form-group">

						<input
							className="form-control"
							onChange={ this.handleChange.bind( this, "searchText" ) }
							placeholder="Search For Friends"
							value={ this.state.searchText }
						/>

						<select
							className="input-medium"
							onChange={ this.handleChange.bind( this, "orderBy" ) }
							value={ this.state.orderBy }
						>
							<option value="name">Name</option>
							<option value="friend_count">#Friends</option>
						</select>

						<select
							className="input-medium"
							onChange={ this.handleChange.bind( this, "order" ) }
							value={ this.state.order }
						>
							<option value={ "descending" }>Descending</option>
							<option value={ "ascending" }>Ascending</option>
						</select>

					</div>
				</form>

				<ul>
				</ul>
			</div>
		);
	}
}

export default FriendsList;
```

___

### Step 3: Friend Component, repeating, filtering.

Create a new file named `Friend.js` and follow the usual steps for creating a class with a render method that returns the following JSX:

``` jsx
<li className='friend'>
	<img className="profile-pic" src='http://placebear.com/50/50.jpg' />

		<h3>Cali Fornia</h3>

		<div className="location">
			Location: New Port Beach, California, United States
		</div>

		<div className="status">
			Status: I hate the snow. I wish I was on the beach right now!!! <span className="hashtag">#ihateprovo</span>
		</div>

		<div className="num-friends">
			Friends: 1,367
		</div>
</li>
```

* Import `Friend` into `FriendsList` and place it inside the `ul` tag at the bottom.
	* You should now see a single friend listed, but we want to display our whole list!
* First we need our data, import `friends.js` into `FriendList.js` and save it to a variable named `friends`.
* At the top of the render method `map` over the array of friends to create an array of `Friend` components, passing in `friend.name`, `friend.pic_square`, `friend.status`, `friend.friend_count`, and `friend.current_location` as props.
	* Don't forget that every repeated item in React needs a unique `key`. `friend.$$hashKey` would work well for this.
	* Be careful of null values!
* Adjust `Friend.js` to use `this.props` instead of the static data we included in our original JSX.

**Checkpoint:** You should now be displaying a large list of friends. Your code should look something like this:

``` jsx
// FriendsList.js
import React from "react";

import friends from "../../friends";

import Friend from "./Friend";

class FriendsList extends React.Component {

// ...
    render() {
		const friendsList = friends.map( friend => (
			<Friend
				currentLocation={ friend.current_location || {} }
				friendCount={ friend.friend_count }
				key={ friend.$$hashKey }
				name={ friend.name }
				pictureUrl={ friend.pic_square }
				status={ friend.status ? friend.status.message : "" }
			/>
		) );

        return (
            // ...
            <ul>
                { friendsList }
            </ul>
        );
    }
    // ...
```

``` jsx
// Friend.js
import React from "react";

class Friend extends React.Component {
	render() {
		return (
			<li className='friend'>
				<img className="profile-pic" src={ this.props.pictureUrl } />

					<h3>{ this.props.name }</h3>

					<div className="location">
						Location: { this.props.currentLocation.city }, { this.props.currentLocation.state }, { this.props.currentLocation.country }
					</div>

					<div className="status">
						{ this.props.status }
					</div>

					<div className="num-friends">
						{ this.props.friendCount }
					</div>
			</li>
		);
	}
}

export default Friend;
```

As the final touch, we need to add sorting and filtering. For this we will use plain JavaScript.

* Using the values we have stored on our FriendList component's state and built in array methods sort, filter, and reverse the array of Friend components as expected.
    * **Warning:** JavaScript's built in `.sort` does not reliably sort in Chrome. Either test in another browser or find a different sorting algorithm.
    * Your code should look something like this:

``` jsx
// FriendsList.js

// ...
const friendsList = friends
	.filter( friend => friend.name.toLowerCase().indexOf( this.state.searchText.toLowerCase() ) !== -1 )
	.sort( ( a, b ) => a[ this.state.orderBy ] > b[ this.state.orderBy ] )
	.map( friend => (
		<Friend
			currentLocation={ friend.current_location || {} }
			friendCount={ friend.friend_count }
			key={ friend.$$hashKey }
			name={ friend.name }
			pictureUrl={ friend.pic_square }
			status={ friend.status ? friend.status.message : "" }
		/>
	) );

const displayFriends = this.state.order === "ascending" ? friendsList : friendsList.slice().reverse();

// ...
```

___

### Black Diamonds:

* Currently we are only searching by name. Create a select that allows users to choose what to search by.
* Update the UI so that Friend components without location data do not display two empty commas.


## Contributions

### Contributions

####

If you see a problem or a typo, please fork, make the necessary changes, and create a pull request so we can review your changes and merge them into the master repo and branch.

## Copyright

### Copyright

####

© DevMountain LLC, 2016. Unauthorized use and/or duplication of this material without express and written permission from DevMountain, LLC is strictly prohibited. Excerpts and links may be used, provided that full and clear credit is given to DevMountain with appropriate and specific direction to the original content.

<img src="https://devmounta.in/img/logowhiteblue.png" width="250">
