---
layout: post
title:      "'Flatiron Project #5: The Capstone'"
date:       2021-03-25 16:28:00 -0400
permalink:  flatiron_project_5_the_capstone
---

Maaan, oh man. It sure has been a ride.

From Ruby to JavaScript to React/Redux, i've learned different languages, syntaxes, frameworks, and backend + frontend programming. Even though i started this bootcamp with no technical background whatsoever, I can OFFICIALLY call myself a full stack software engineer! Its amazing to see how the multiple pieces of logic learned across all 5 modules of the course have come together in my final capstone project. Although intimidating, React and Redux together are a powerful combination when building full scale web applications.

My final project is a web application called Adventure Time. Adventure Time is a hiking app that allows users to submit trails they visit in Colorado and keep track of their favorite ones. Users are able to explore our beautiful state and record things like the intensity level of the hike and the length of time they spent on it. Adventure Time 2.0(coming soon!) will allow users to add pictures so that they can really encapsulate the memories.  

I would say that one of the most important things i took away from this project was the Redux data flow which works most simplistically like this: Actions => Reducers => Updated State. 

1) Action Creators are just normal JavaScript functions that accept 2 arguments: a type and a payload. We can use action creator functions to dispatch an action to a reducer. See the example below:
```
export const fetchTrails = () => {
    return (dispatch) => {
        fetch("http://localhost:3000/trails")
        .then(resp => resp.json())
        .then(trails => dispatch({type: 'FETCH_TRAILS', payload: trails}))
    }
}
```

2) A Reducer is a function that determines changes to an application's state and uses the action it receives to determine the change. Ex: 
```
export const trailsReducer = (state = [], action) => {
    switch(action.type){
        case 'FETCH_TRAILS':
            return action.payload
        default:
            return state
    }
}
```
As you a can see above, reducers find the proper action to execute by matching the **type** argument from the action dispatched, to the **case** statement inside of the reducer function.

3) The state is updated by returning the data passed in from the action's payload key which in this case, is all of the trails fetched from the rails API backend. If we wanted to do something with that data before updating the state we can! Let's take a look at how by following the flow of how a trail is deleted from my app.

Firstly, I created a delete button in my TrailsList Component that triggers the handleClick function when clicked.
```
<Button><button id={trail.id} onClick={handleClick}>Delete Trail</button></Button>
```

The handleClick() function parses the integer of the number from the button clicked(every trail has a unique key). That id is then passed into the handleClick function where the deleteTrail action creator is available to us. 
```const handleClick = event => {
        const id = parseInt(event.target.id)
        deleteTrail(id)
        history.push('/trails')
    }
```

One concept that is that is VERY important to understand, is that the { connect } component made avalable to us by react-redux. The connect() function connects a React component to a Redux store. It accepts mapStateToProps(if not being used, pass in null as the argument), mapDispatchToProps, and the component it is being connected to. The mapDispatch to props argument is where we can pass in the action creator we need out component to have access to. In this case, its deleteTrail. 
```
export default connect(mapStateToProps, { deleteTrail })(TrailsList)
```

Because we passed in the deleteTrail action, we now have access to that fuction in our component. Below is my deleteTrail action which accepts the trails id(trailId) and sends that data to the reducer function as the payload :
```
export default function deleteTrail(trailId){
    return (dispatch) => {
        return fetch(`http://localhost:3000/trails/${trailId}`, {
            method: "DELETE",
            headers: {
                "Content-Type": "application/json"
            }
        })
        .then(res => dispatch({type: 'DELETE_TRAIL', payload: trailId})
        )
        .catch(error => console.error(error))
    }
}
```

In my DELETE_TRAIL reducer, I filter through the state and return all trails passed in excluding the id that matched the trail clicked. See how the case below matches the type dispatched from my action?
```
export const trailsReducer = (state = [], action) => {
    switch(action.type){
        case 'FETCH_TRAILS':
            return action.payload 
        case 'DELETE_TRAIL':
            return state.filter(trail => trail.id !== action.payload)
        default:
            return state
    }
}
```

My state has now been updated and the user no longer sees the deleted trail on the frontend. That is just a quick overview of the React Redux flow. The next step for me will be learning about React hooks so i can stay up to date!
