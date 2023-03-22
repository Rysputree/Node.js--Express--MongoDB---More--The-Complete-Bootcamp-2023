

# 🚧 Node.js, Express, MongoDB & More: The Complete Bootcamp 2023

 💥 Master Node by building a real-world RESTful API and web app (with authentication, Node.js security, payments & more)

# 🚧Building API

## Initialization Project

```js
npm init
npm i express@4
```

## Setup basics

```js
const express = require("express");

//Get an instance (object) of the server
const app = express();

app.get("/", (req, res) => {
  res
    .status(200)
    .json({ message: "Hello from the server side!", app: "itsyuimoriiTours" });
});

//Start the server
const port = 3000;
app.listen(port, (error) => {
  if (!error)
    console.log(
      `Server is Successfully Running, and App is listening on port ${port}...`
    );
  else console.log("Error occurred, server can't start", error);
});
```

## 💛APIS and Restful API design

**Application Programming Interface**: a piece of software that can be used by another piece of software, in order to allow applications to talk to each other.

**Get vs post**

![](https://res.cloudinary.com/dxmfrq4tk/image/upload/v1678301639/node.js%20notes/Screen_Shot_2023-02-21_at_9.31.31_PM_brp11u.png)

### The REST Architecture

1. Separate API into logical **resources**
2. Expose structured, **resource-based URLs**
3. **Use HTTP methods(verbs)**
4. Send data as **JSON** (usually)
5. Be **stateless**
   > **Resource**: Object or representation of something, whichhas data associated to it. Any information that can benamed can be a resource.

- URL: `https://www.itsyuimoriiTours.com/addNewTour`

  _Endpoints should containonly resources (nouns),and use HTTP methods for actions!_

![img](https://res.cloudinary.com/dxmfrq4tk/image/upload/v1678300655/node.js%20notes/Screen_Shot_2023-03-08_at_12.36.58_PM_czrzp0.png)

The ability to **update resources**. And for that,either a `PUT` or a `PATCH `request should be made to the _endpoint_. The difference between them is that with

- **PUT**, the client is supposed to send the entire updated object

- **PATCH**,it is supposed to send only the part of the objectthat has been changed.

- But both of them have the ability to **send data to the server.**A bit like POST, actually, but with a different intent.

- **POST is to create a new resource,** while PUT or PATCH are used to **update an existing resource** and so it then makes all the difference.

- **DELETE** HTTP method.Again, the I.D. or some other unique identifier of the resource to be deleted should be part of the URL.

- If the /tours endpoint is accessed with **GET,** we send data to the client.

  But if the same endpoint is accessed with **POST,** we expect data to come in with a request.

So these are the five `HTTP methods` that we can and should respond to when building our RESTful APIs. so that the client can perform the four basic CRUD operations.So **CRUD stands for Create, Read, Update and Delete.**And you will see this term all the time related to APIs and database stuff.And you see that these HTTP methods map quite nicely to the basic CRUD operations.Now, there might be actions that are not CRUD,and in that case,we just need to get creative with our inputs.

For example,a log in or a search operation,these are not really related to a particular resourceand they're not CRUD operations either,but we still can create endpoints for them.For example, like /login or /search.But we'll talk more about these cases later in practice.

And just to finish this part now,remember that we had two other crazy endpoint namesin the last slide which kind of involvedtwo resources at the same time, right?And that's also no problem at all with REST.

So,` /getToursByUser` can simply be translated to`/users/tours`,in this case, user number three.So this particular endpoint herecould send data about all the toursthat user number three has booked.Makes sense? Or in the case of deleting,there could be a delete request to the sameor a very similar endpoint,requesting tour number nine to be deletedfrom user number three, okay?So there really are a tons of possibilitiesof combining resources like this.But of course, we don't have to implementall these combinations in our API.We only implement what makes sensein the case of our applicationand the client that wants to consume our API.So, this is how we make use of HTTP methodsto build user-friendly and nicely structured URLsthat are easy and logical to consume for the client.

Now, about the data that the client actually receives,or that the server receives from the client,usually, we use the JSON Data Format.And so let's briefly learn what JSON actually isand how to format our API responses.

JSON is a very lightweight data interchange formatwhich is heavily used by web APIscoded in any programming language.So it's just not related to a JavaScript.

And it's so widely used todaybecause it's really easy for both humans and computersto understand and write JSON.

So you're probably already noticing that JSON looks a bit like a regular JavaScript object, right? With all these key-value pairs.There are, however, some differences,and the most important one is that all the keyshave to be strings.It's also very typical for the valuesto be strings as well but they can be other thingslike numbers, true or false values, other object,or even arrays of other values.It's quite straighforward, actually.And from this example,you can kind of see how some typical JSON might look like.Let's say that this is a data that we have in our databasefor a GET request to this URLso the tour with I.D. of five.Now, we could send it back like this to the client,but we usually do some simple response formattingbefore sending.There are a couple of standards for thisand we're gonna use a very simple one called Jsend.We simply create a new object,then add a status message to itin order to inform the client whether the requestwas a success, fail or error,and then we put our original datainto a new object called Data, okay?And we can develop this even a bit furtherbut this is really the simplest wayof formatting with Jsend.And just, by the way,wrapping the data into an additional objectlike we did here is called Enveloping,and it's a common practiceto mitigate some security issues and other problems.Also, there are other standardsfor response-formatting that you can look into,like Jsend:API or the Odata JSON Protocol.Alright,

![stateless](https://res.cloudinary.com/dxmfrq4tk/image/upload/v1678302421/node.js%20notes/Screen_Shot_2023-03-08_at_1.06.56_PM_tdoz3h.png)

and finally,a RESTful API should always be stateless.So, what does stateless actually mean?Well, in a stateless RESTful API,all state is handled on the clientand not on the server.And state simply refers to a piece of datain the application that might change over time.For example,whether a certain user is logged inor on a page with a list with several pages,what the current page is.Now the fact that the state should be handledon the client means that each request must containall the information that is necessary to processa certain request on the server, alright?Does that make sense? So, the server should never ever have to remember the previous request in order to processthe current request.Let's take the list with several pages as an example.And let's say that recurrently on page fiveand want to move forward to page six.So we could have a simple endpoint called /tours/nextPageand submit a request to it, right?But the server would then have to figure outwhat the current page is and based on thatsend the next page to the client.In other words,the server would have to remember the previous request.It would have to handle the state server sideand that is exactly what we want to avoid in RESTful APIs, okay?Instead, in this case,we should create a /tours/page endpointand paste the number six to itin order to request page number six.This way, we would then state on the clientbecause on a client,we would already know that we're on page fiveand so all we had to do is to just add oneand then request page number six.So the server doesn't haveto remember anything in this case.All it has to do is to send back datafor page number six as we requested.And by the way, statelessness and statefulness,which is the opposite,are very important concepts in computer scienceand application design in general.So, it's a good idea to actually have some understandingwhat a stateless API is and how it works.Anyway, this was a huge lecture,but also one of the most important ones.I cannot stress that enough and I actually thinkthat you can see that, right?Anyway, let's now finally get back to our code.

## 💛Starting design Restful API

### handling GET request

```js
const tours = JSON.parse(
  fs.readFileSync(`${__dirname}/dev-data/data/tours-simple.json`)
);

app.get("/api/v1/tours", (req, res) => {
  res.status(200).json({
    //JSend data formations
    status: "success",
    data: {
      //   tours: tours,
      tours,
    },
  });
});
```

> https://127.0.0.1:3000/api/v1/tours

![](https://res.cloudinary.com/dxmfrq4tk/image/upload/v1678334669/node.js%20notes/Screen_Shot_2023-03-08_at_10.03.30_PM_wzxz93.png)

### handling POST request

remember that with a post request, we can **send data from the client to the server**, This data is then ideally available on the request. The request object again is what holds all the data, all the information, about the request that was done.

**Middleware**: It's called middleware because it stands between,so in the middle of the request and the response. And the step the requests go through, in this example is simply that the data from the body is added to it.So it's added to the request object by using this middleware.

```js
app.post("/api/v1/tours", (req, res) => {
  //the data from the body in the console, just to verify that it actually works, so req.body.
  //get data from the body in the console
  console.log(req.body);
  res.send("Done");
});
```

![](https://res.cloudinary.com/dxmfrq4tk/image/upload/v1678335558/node.js%20notes/Screen_Shot_2023-03-08_at_10.17.54_PM_ohsgwf.png)

### Persist the data into JSON file(fictional database)

when we create a new object, we never specify the id of the object. The database usually takes care of that.

```js
app.post("/api/v1/tours", (req, res) => {
  //the data from the body in the console, just to verify that it actually works, so req.body.
  //get data from the body in the console
  console.log(req.body);
  const newId = tours[tours.length - 1].id + 1;
  //create a new object by merging two existing objects
  const newTour = Object.assign({ id: newId }, req.body);
  res.push(newTour);
  res.send("Done");
});
```

why use fs.writefile instead of fs.writefileSync ?

We are inside of a call-back function, that is gonna run in the event loop. We can never, ever block the event loop.What we're gonna do is to use writeFile and not to Sync in this one. We want to pass in a call-back function that is gonna be processed in the background and as soon as it's ready, t's gonna put its event in one of the event loop queue, which is then gonna be handled as soon as the event loop passes that phase. Anyway, let's get the file name from up here, because we will really override this file o that when we restart this server, it's then gonna be there

```js
const tours = JSON.parse(
  fs.readFileSync(`${__dirname}/dev-data/data/tours-simple.json`)
);

app.post("/api/v1/tours", (req, res) => {
  //the data from the body in the console, just to verify that it actually works, so req.body.
  //get data from the body in the console
  //console.log(req.body);
  const newId = tours[tours.length - 1].id + 1;
  //create a new object by merging two existing objects
  const newTour = Object.assign({ id: newId }, req.body);
  //push this tour into the tour array
  tours.push(newTour);
  //persist that into file,using fs.writeFileSync
  fs.writeFile(
    `${__dirname}/dev-data/data/tours-simple.json`,
    JSON.stringify(tours),
    (err) => {
      res.status(201).json({
        status: "success",
        data: {
          tour: newTour,
        },
      });
    }
  );
  // res.send('Done');
  //Error [ERR_HTTP_HEADERS_SENT]: Cannot set headers after they are sent to the client
});
```

![image](https://res.cloudinary.com/dxmfrq4tk/image/upload/v1678337054/node.js%20notes/Screen_Shot_2023-03-08_at_10.42.12_PM_ncb1sn.png)

> Convert javascript object into JSON, using `JSON.stringigy`

### Responding to URL Parameters

```js
app.get('/api/v1/tours/:id', (req, res) => {
  // where all the parameters of all the variables that we define here are stored
  console.log(req.params);
}
```

> postman `127.0.0.1:3000/api/v1/tours/5`
> console.log `{id : "5"}`

```js
const id = req.params.id * 1;
```

- when we multiply a string that looks like a number,when we multiply that with another number,it will then automatically convert that string to a number.

- loop through the array,and in each of the iterations,we will have access to the current element,and we will return either true or false in each of the iterations

```js
const tour = tours.find((el) => el.id === id);
```

Now what the find method will then dois that basically, it will create an arraywhich only contains the elementwhere this comparison here turns out to be true, all right?And in this situation, we want to find the elementwhere the ID is equal to the onethat we get from the parameters.And so by specifying this callback function herewith this comparison, we will ensurethat only the element where the ID is actually equalto the specified ID in the parameterswill get returned from the find methodand stored into tour, all right?

> ![image](https://res.cloudinary.com/dxmfrq4tk/image/upload/v1678340395/node.js%20notes/Screen_Shot_2023-03-08_at_11.39.27_PM_sokdhl.png)

```js
app.get("/api/v1/tours", (req, res) => {
  res.status(200).json({
    //JSend data formations
    status: "success",
    results: tours.length,
    data: {
      //   tours: tours,
      tours: tours,
    },
  });
});

app.get("/api/v1/tours/:id", (req, res) => {
  // where all the parameters of all the variables that we define here are stored
  console.log(req.params);
  //when we multiply a string that looks like a number,when we multiply that with another number,it will then automatically convert that string to a number.
  const id = req.params.id * 1;

  //loop through the array,and in each of the iterations,we will have access to the current element,and we will return either true or false in each of the iterations
  const tour = tours.find((el) => el.id === id);

  //check if the id is not existing
  if (!tour) {
    return res.status(404).json({
      status: "Not Found",
      message: "Invalid ID",
    });
  }

  /* if 127.0.0.1:3000/api/v1/tours/23, console.log
{
    "status": "Not Found",
    "message": "Invalid ID"
}
*/

  res.status(200).json({
    //JSend data formations
    status: "success",
    data: {
      //   tours: tours,
      tour: tour,
    },
  });
});
```

### handling PATCH request

```js
app.patch("/api/v1/tours/:id", (req, res) => {
  //check if the id is not existing
  if (req.params.id * 1 > tours.length) {
    return res.status(404).json({
      status: "failed",
      message: "Invalid ID",
    });
  }
  res.status(200).json({
    status: "success",
    data: {
      tour: "<Updated tour here...>",
    },
  });
});
```

### handling DELETE request

```js
app.delete("/api/v1/tours/:id", (req, res) => {
  //check if the id is not existing
  if (req.params.id * 1 > tours.length) {
    return res.status(404).json({
      status: "failed",
      message: "Invalid ID",
    });
  }
  res.status(204).json({
    status: "success",
    data: null,
  });
});
```

### ✨Stage 1: all code

```js
const express = require("express");
const fs = require("fs");

//Get an instance (object) of the server
const app = express();

app.use(express.json());

// app.get('/', (req, res) => {
//   res
//     .status(200)
//     .json({ message: 'Hello from the server side!', app: 'itsyuimoriiTours' });
// });
const tours = JSON.parse(
  fs.readFileSync(`${__dirname}/dev-data/data/tours-simple.json`)
);

app.get("/api/v1/tours", (req, res) => {
  res.status(200).json({
    //JSend data formations
    status: "success",
    results: tours.length,
    data: {
      //   tours: tours,
      tours: tours,
    },
  });
});

app.get("/api/v1/tours/:id", (req, res) => {
  // where all the parameters of all the variables that we define here are stored
  console.log(req.params);
  //when we multiply a string that looks like a number,when we multiply that with another number,it will then automatically convert that string to a number.
  const id = req.params.id * 1;

  //loop through the array,and in each of the iterations,we will have access to the current element,and we will return either true or false in each of the iterations
  const tour = tours.find((el) => el.id === id);

  //check if the id is not existing
  if (!tour) {
    return res.status(404).json({
      status: "Not Found",
      message: "Invalid ID",
    });
  }

  res.status(200).json({
    //JSend data formations
    status: "success",

    data: {
      //   tours: tours,
      tour: tour,
    },
  });
});

app.post("/api/v1/tours", (req, res) => {
  //the data from the body in the console, just to verify that it actually works, so req.body.
  //get data from the body in the console
  //console.log(req.body);
  const newId = tours[tours.length - 1].id + 1;
  //create a new object by merging two existing objects
  const newTour = Object.assign({ id: newId }, req.body);
  //push this tour into the tour array
  tours.push(newTour);
  //persist that into file,using fs.writeFileSync
  fs.writeFile(
    `${__dirname}/dev-data/data/tours-simple.json`,
    JSON.stringify(tours),
    (err) => {
      res.status(201).json({
        status: "success",
        data: {
          tour: newTour,
        },
      });
    }
  );
  //res.send('Done');
});

app.patch("/api/v1/tours/:id", (req, res) => {
  //check if the id is not existing
  if (req.params.id * 1 > tours.length) {
    return res.status(404).json({
      status: "failed",
      message: "Invalid ID",
    });
  }
  res.status(200).json({
    status: "success",
    data: {
      tour: "<Updated tour here...>",
    },
  });
});

app.delete("/api/v1/tours/:id", (req, res) => {
  //check if the id is not existing
  if (req.params.id * 1 > tours.length) {
    return res.status(404).json({
      status: "failed",
      message: "Invalid ID",
    });
  }
  res.status(204).json({
    status: "success",
    data: null,
  });
});

//Start the server
const port = 3000;
app.listen(port, (error) => {
  if (!error)
    console.log(
      `Server is Successfully Running, and App is listening on port ${port}...`
    );
  else console.log("Error occurred, server can't start", error);
});
```

### Refactoring ROUTES

```js
const getAllTours = (req, res) => {
  res.status(200).json({
    //JSend data formations
    status: "success",
    results: tours.length,
    data: {
      //   tours: tours,
      tours: tours,
    },
  });
};

app.get("/api/v1/tours", getAllTours);
```

```js
app.get("/api/v1/tours", getAllTours);
app.post("/api/v1/tours", createTour);
app.get("/api/v1/tours/:id", getTour);
app.patch("/api/v1/tours/:id", updateTour);
app.delete("/api/v1/tours/:id", deleteTour);

//optimize code
app.route("/api/v1/tours").get(getAllTours).post(createTour);

app
  .route("/api/v1/tours/:id")
  .get(getTour)
  .patch(createTour)
  .delete(deleteTour);
```

### Middleware and the Request-Response circle

In fact, we have used middleware before. We use Express to point JSON to access the request body on the request object (body.parser). In fact, we can say that in Express, everything is middleware.**The initial request and response object progressively traverses each middleware.**

### Create Middleware function

```js
//create own middleware
app.use((req, res, next) => {
  console.log("Hello from the middleware 👋 ");
  next();
});

app.use((req, res, next) => {
  req.requestTime = new Date().toDateString();
  next();
});
const getAllTours = (req, res) => {
  res.status(200).json({
    //JSend data formations
    status: "success",
    requestedAt: req.requestTime,
    results: tours.length,
    data: {
      //   tours: tours,
      tours: tours,
    },
  });
};
```

> Output: `"requestedAt": "Thu Mar 09 2023",`

### Using 3rd-Party Middleware

[morgan github 🐈‍⬛](https://github.com/expressjs/morgan/blob/master/index.js)

```js
const morgan = require("morgan");
app.use(morgan("dev"));
```

> GET /api/v1/tours 200 4.770 ms - 8885

### Implementing the USER Routes

```js
const getAllUsers = (req, res) => {
  res.status(500).json({
    status: "error",
    message: "this route is not yet defined!",
  });
};

const createUser = (req, res) => {
  res.status(500).json({
    status: "error",
    message: "this route is not yet defined!",
  });
};

const getUser = (req, res) => {
  res.status(500).json({
    status: "error",
    message: "this route is not yet defined!",
  });
};

const updateUser = (req, res) => {
  res.status(500).json({
    status: "error",
    message: "this route is not yet defined!",
  });
};
const deleteUser = (req, res) => {
  res.status(500).json({
    status: "error",
    message: "this route is not yet defined!",
  });
};

//UserRoutes
app.route("/api/v1/users").get(getAllUsers).post(createUser);

app
  .route("/api/v1/users/:id")
  .get(getUser)
  .patch(updateUser)
  .delete(deleteUser);
```

### Creating and Mounting Multiple Routers

```js
//Mounting Routes
const tourRouter = express.Router();
const userRouter = express.Router();

app.use("/api/v1/tours", tourRouter);
app.use("/api/v1/users", userRouter);

tourRouter.route("/").get(getAllTours).post(createTour);
tourRouter.route("/:id").get(getTour).patch(createTour).delete(deleteTour);

userRouter.route("/").get(getAllUsers).post(createUser);
userRouter.route("/:id").get(getUser).patch(updateUser).delete(deleteUser);
```

### ✨Stage 2:Better File structure

> app.js
>
> ```js
> const express = require("express");
> const morgan = require("morgan");
>
> const tourRouter = require("./Routes/tourRoutes");
> const userRouter = require("./Routes/userRoutes");
>
> //Get an instance (object) of the server
> const app = express();
>
> // 1) MIDDLEWARE
>
> app.use(morgan("dev"));
> app.use(express.json());
> app.use((req, res, next) => {
>   console.log("Hello from the middleware 👋 ");
>   next();
> });
> app.use((req, res, next) => {
>   req.requestTime = new Date().toDateString();
>   next();
> });
>
> // 3) ROUTES
> app.use("/api/v1/users", userRouter);
> app.use("/api/v1/tours", tourRouter);
>
> //app.get('/api/v1/tours', getAllTours);
> //app.post('/api/v1/tours', createTour);
> // app.get('/api/v1/tours/:id', getTour);
> // app.patch('/api/v1/tours/:id', updateTour);
> // app.delete('/api/v1/tours/:id', deleteTour);
>
> module.exports = app;
> ```

> Routes/tourRoutes.js
>
> ```js
> const express = require("express");
>const tourController = require("./../controllers/tourController");
> const router = express.Router();
>
> router
>.route("/")
> .get(tourController.getAllTours)
>   .post(tourController.createTour);
>   router
>   .route("/:id")
> .get(tourController.getTour)
>   .patch(tourController.createTour)
>   .delete(tourController.deleteTour);
>   
>   module.exports = router;
>```

> Routes/userRoutes.js
>
> ```js
> const express = require("express");
> const userController = require("./../controllers/userController");
> const router = express.Router();
>
> router
>   .route("/")
>   .get(userController.getAllUsers)
>   .post(userController.createUser);
> router
>   .route("/:id")
>   .get(userController.getUser)
>   .patch(userController.updateUser)
>   .delete(userController.deleteUser);
>
> module.exports = router;
> ```

> controllers/tourController.js
>
> ```js
> const fs = require("fs");
>
> const tours = JSON.parse(
>   fs.readFileSync(`${__dirname}/../dev-data/data/tours-simple.json`)
> );
>
> exports.checkID = (req, res, next, val) => {
>   //check if the id is not existing
>   console.log(`Tour id is: ${val}`);
>   if (req.params.id * 1 > tours.length) {
>     return res.status(404).json({
>       status: "fail",
>       message: "Invalid ID",
>     });
>   }
>   next();
> };
>
> exports.checkBody = (req, res, next) => {
>   if (!req.body.name || !req.body.price) {
>     return res.status(400).json({
>       status: "fail",
>       message: "Missing name or price",
>     });
>   }
>   next();
> };
>
> // 2) ROUTE HANDLER
> exports.getAllTours = (req, res) => {
>   res.status(200).json({
>     //JSend data formations
>     status: "success",
>     requestedAt: req.requestTime,
>     results: tours.length,
>     data: {
>       //   tours: tours,
>       tours: tours,
>     },
>   });
> };
>
> exports.getTour = (req, res) => {
>   // where all the parameters of all the variables that we define here are stored
>   console.log(req.params);
>   //when we multiply a string that looks like a number,when we multiply that with another number,it will then automatically convert that string to a number.
>   const id = req.params.id * 1;
>
>   //loop through the array,and in each of the iterations,we will have access to the current element,and we will return either true or false in each of the iterations
>   const tour = tours.find((el) => el.id === id);
>
>   //check if the id is not existing
>   // if (!tour) {
>   //   return res.status(404).json({
>   //     status: 'Not Found',
>   //     message: 'Invalid ID',
>   //   });
>   // }
>
>   res.status(200).json({
>     //JSend data formations
>     status: "success",
>
>     data: {
>       //   tours: tours,
>       tour: tour,
>     },
>   });
> };
>
> exports.createTour = (req, res) => {
>   //the data from the body in the console, just to verify that it actually works, so req.body.
>   //get data from the body in the console
>   //console.log(req.body);
>   const newId = tours[tours.length - 1].id + 1;
>   //create a new object by merging two existing objects
>   const newTour = Object.assign(`{ id: newId }`, req.body);
>   //push this tour into the tour array
>   tours.push(newTour);
>   //persist that into file,using fs.writeFileSync
>   fs.writeFile(
>     `${__dirname}/../dev-data/data/tours-simple.json`,
>     JSON.stringify(tours),
>     (err) => {
>       res.status(201).json({
>         status: "success",
>         data: {
>           tour: newTour,
>         },
>       });
>     }
>   );
>   //res.send('Done');
> };
>
> exports.updateTour = (req, res) => {
>   //check if the id is not existing
>   // if (req.params.id * 1 > tours.length) {
>   //   return res.status(404).json({
>   //     status: 'failed',
>   //     message: 'Invalid ID',
>   //   });
>   // }
>   res.status(200).json({
>     status: "success",
>     data: {
>       tour: "<Updated tour here...>",
>     },
>   });
> };
>
> exports.deleteTour = (req, res) => {
>   // //check if the id is not existing
>   // if (req.params.id * 1 > tours.length) {
>   //   return res.status(404).json({
>   //     status: 'failed',
>   //     message: 'Invalid ID',
>   //   });
>   // }
>   res.status(204).json({
>     status: "success",
>     data: null,
>   });
> };
> ```

> controllers/userController.js
>
> ```js
> // 2) ROUTE HANDLER
> exports.getAllUsers = (req, res) => {
>   res.status(500).json({
>     status: "error",
>     message: "this route is not yet defined!",
>   });
> };
>
> exports.createUser = (req, res) => {
>   res.status(500).json({
>     status: "error",
>     message: "this route is not yet defined!",
>   });
> };
>
> exports.getUser = (req, res) => {
>   res.status(500).json({
>     status: "error",
>     message: "this route is not yet defined!",
>   });
> };
>
> exports.updateUser = (req, res) => {
>   res.status(500).json({
>     status: "error",
>     message: "this route is not yet defined!",
>   });
> };
> exports.deleteUser = (req, res) => {
>   res.status(500).json({
>     status: "error",
>     message: "this route is not yet defined!",
>   });
> };
> ```

> server.js
>
> ```js
> const app = require("./app");
>
> // 4) START SERVER
> const port = 3000;
> app.listen(port, (error) => {
>   if (!error)
>     console.log(
>       `Server is Successfully Running, and App is listening on port ${port}...`
>     );
>   else console.log("Error occurred, server can't start", error);
> });
> ```

### Setup server.js

```bash
    "start": "nodemon server.js"
    npm start

    //install nodemon
    npm i nodemon --save-dev
```

### Param middleware

So param middleware is middleware that only runs for certain parameters, so basically, when we have a certain parameter in our URL. Now in our example here, the only parameter that we might have in our route URL is the id, right?

> ```js
>router.route("/:id");
> ```
> 
> ```js
>router.param("id", (req, res, next, val) => {
> console.log(`Tour id is: ${val}`);
> next();
>   });
>   ```

Using it to check valid ID

> Routes/tourRoutes.js

```js
router.param("id", tourController.checkID);

// router.param('id', (req, res, next, val) => {
//   //console.log(`Tour id is: ${val}`);
//   next();
// });
```

> ontrollers/tourController.js

```js
exports.checkID = (req, res, next, val) => {
  //check if the id is not existing
  console.log(`Tour id is: ${val}`);
  if (req.params.id * 1 > tours.length) {
    return res.status(404).json({
      status: "fail",
      message: "Invalid ID",
    });
  }
  next();
};
```

### Chaining multiple middleware functions for same route

> Routes/tourRoutes.js

if its post request, it will run this middleware => tourController.checkBody

```js
router
  .route("/")
  .get(tourController.getAllTours)
  .post(tourController.checkBody, tourController.createTour);
```

> controllers/tourController.js

```js
exports.checkBody = (req, res, next) => {
  if (!req.body.name || !req.body.price) {
    return res.status(400).json({
      status: "fail",
      message: "Missing name or price",
    });
  }
  next();
};
```

### Servering static files

```js
app.use(express.static(`${__dirname}/public`));
```

> http://127.0.0.1:3000/overview.html
>
> ```js
> //npm start
> [nodemon] 2.0.21
> [nodemon] to restart at any time, enter `rs`
> [nodemon] watching path(s): *.*
> [nodemon] watching extensions: js,mjs,json
> [nodemon] starting `node server.js`
> Server is Successfully Running, and App is listening on port 3000...
> GET /overview.html 304 4.760 ms - -
> GET /css/style.css 304 0.755 ms - -
> GET /img/logo-white.png 304 1.210 ms - -
> Hello from the middleware 👋
> GET /img/user.jpg 404 3.811 ms - 151
> Hello from the middleware 👋
> GET /img/tour-2-cover.jpg 404 1.937 ms - 159
> Hello from the middleware 👋
> GET /img/tour-4-cover.jpg 404 2.088 ms - 159
> Hello from the middleware 👋
> GET /img/tour-3-cover.jpg 404 2.387 ms - 159
> GET /img/tours/tour-2-cover.jpg 304 2.765 ms - -
> Hello from the middleware 👋
> GET /img/tour-6-cover.jpg 404 1.974 ms - 159
> Hello from the middleware 👋
> GET /img/tour-5-cover.jpg 404 1.699 ms - 159
> GET /img/logo-green.png 304 0.394 ms - -
> GET /img/icons.svg 304 0.280 ms - -
> GET /img/favicon.png 304 0.366 ms -
> ```

### ⁉️ Environment variables

So node JS, or Express apps,can run in different environments.And the most important ones are the **development environmentand the production environment.**That's because depending on the environment,we might use different databases for example,or we might turn login on or off,or we might turn debugging on or off,or really all kinds of different settings that mightchange depending on the development that we're in.So again the most important ones are the **developmentand the production environment.**

> server.js
>
> ```js
> console.log(app.get("env"));
> console.log(process.env);
> ```

> ```js
> NODE_ENV=developement nodemon server.js
> ```

- `npm i dotenv`

- `server.js `

  ```js
  const dotenv = require("dotenv");
  dotenv.config({ path: "./config.env" });
  // console.log(app.get('env'));
  console.log(process.env);
  ```

  ```bash
    NODE_ENV: 'development',
    PORT: '3000',
    USERNAME: 'itsyuimorii✨',
    PASSWORD: '123456'

  ```

- `app.js`

  ```js
  if (process.env.NODE_ENV === "development") {
    app.use(morgan("dev"));
  }
  ```

### ESlint and prettier

> package.json
>
> ```js
> npm i eslint prettier eslint-config-prettier eslint-plugin-prettier eslint-config-airbnb eslint-plugin-node eslint-plugin-import eslint-plugin-jsx-a11y eslint-plugin-react --save-dev
> //they have to be installed locally.It would not work if we tried to do this globally.
>
>
>  "devDependencies": {
>    "eslint": "^8.35.0",
>    "eslint-config-airbnb": "^19.0.4",
>    "eslint-config-prettier": "^8.7.0",
>    "eslint-plugin-import": "^2.27.5",
>    "eslint-plugin-jsx-a11y": "^6.7.1",
>    "eslint-plugin-node": "^11.1.0",
>    "eslint-plugin-prettier": "^4.2.1",
>    "eslint-plugin-react": "^7.32.2",
>    "prettier": "^2.8.4"
> ```

> .eslintrc.json
>
> ```js
>  {
>   "extends": ["airbnb", "prettier", "plugin:node/recommended"],
>   "plugins": ["prettier"],
>   "rules": {
>     "prettier/prettier": "error",
>     "spaced-comment": "off",
>     "no-console": "off",
>     "consistent-return": "off",
>     "func-names": "off",
>     "object-shorthand": "off",
>     "no-process-exit": "off",
>     "no-param-reassign": "off",
>     "no-return-await": "off",
>     "no-underscore-dangle": "off",
>     "class-methods-use-this": "off",
>     "prefer-destructuring": ["error", { "object": true, "array": false }],
>     "no-unused-vars": ["error", { "argsIgnorePattern": "req|res|next|val" }]
>   }
> }
> ```

# 🚧Introduction to MongoDB

“MongoDB is a document database with the scalability and flexibility that you want with the querying and indexing that you need” `database  -> colllections -> documents`

👉 **Document based:** MongoDB stores data in documents (field-value pair data structures, NoSQL);

👉 **Scalable:** Very easy to distribute data across multiple machines as your users and amount of data grows;

👉 **Flexible:** No document data schema required, so each document can have different number and type of fields;

👉 **Performant:** Embedded data models, indexing, sharding, flexible documents, native duplication, etc.

👉 **Free and open-source,** published under the SSPL License.

![https://res.cloudinary.com/dxmfrq4tk/image/upload/v1678463656/node.js%20notes/Screen_Shot_2023-03-10_at_9.53.41_AM_bxrsm4.png](https://res.cloudinary.com/dxmfrq4tk/image/upload/v1678463656/node.js notes/Screen_Shot_2023-03-10_at_9.53.41_AM_bxrsm4.png)

Eg: So just imagine we had a **comments collection** which contained a bunch of comment documents.Each of them could actually look exactly like this,so with an author and with the comment text,but instead of doing that,we include these comments right into the blog post document,so in other words, we `embed the comment documents` right into the` post document` .So this process of `embedding, or de-normalizingas` we can also call it, is basically to include so, to embed, some related data all into one single document.

In this example**, the comments are related to the post**,**and so they are included in the same document**.And this makes a database more `performant` in some situations because this way,it can be easier to _read all the data_ that we need all at once.And this is something that we're gonna talk about a lot when learning about data **modeling**

Now, **the opposite of embedding or de-normalizing,is normalizing,** and that's how the data is always modeled in relational databases. So in that case, it's not possible to embed data,and so **the solution is to create a whole new table for the comments and then join the tables by referencing to the ID field of the comments table.**Now we're not gonna use relational databases in this course, but I believe it's still importantto know the differences if you wanna becomea good back-end developer.

Ttwo more things about **BSON documents**. First, the **maximum size** for each documentis currently 16 MB, but this might increase in the future.And second, each document contains a **unique ID**, which acts as a primary key of that document.It's automatically generated with the object ID data type each time there is a new document,and so we don't have to worry about it.

### Create a local database -mongoshell

```js
use itsyuimoriiTours-test
```

> And so it created that database, and it also switched to it. Okay, and now this blank databaseis ready to receive some data.Now remember that inside a database we have collections,and then each collection has documents in it.
>
> **And the data that we create in the Mongo Shell is always documents.**And so of course we have to `create the document inside of a collection`,

```js
db.tours.insertMany();
```

> and so we **specify that collection before we insert a document**.And this works like this, so db, which stands for the current database, which is in this case `itsyuimoriiTour-test`, and then we **specify the name of the collection** which is tours, and then on that we use the `db.tours.insertMany() `function. so just to recap here, Database is the currently used database that is right now active, and when we want to insert adocument into it we need to specify the collection where that document is gonna live.And we do that by using dot, and then **the name of the collection, which is in this case "tours"**.Now right now this collection hasn't been created,and so it will create it once we run this command

```js
db.tours.insertMany({ name: "The Forest Hiker", price: 297, rating: 4.7 });
```

> So, remember that MongoDb uses **BSON**,which is quite similar to JSON,so we can actually simply pass a **JavaScript object** into this insert Many function, and it willthen convert it into JSON and BSON, so a regular JavaScript object like this,and then just like before, let's definethe name of the tour.And I'm calling it The Forest Hiker, which isone of the tours that we actually have in our JSON document at this point, so The Forest Hiker,let's set a price to 297,and then also a rating, like an average rating,which can be 4.7.
>
> So this here looks like just a regular JavaScript object,now we could also use quotes on the property names,but that is actually optional, okay.Now here on these,and here we probably need the double quotes,okay so JSON does not recognize the single quotes,and so to be on the safe side let's just use the double quotes here, even though I usually like to use the single ones.And now just before we hit return, we actually need toalso correct this function here, because it's notinsertMany, because we're actually only insertingone document here,so it's called insertOne, okay. So we use insertMany to create multiple documents,and we use insertOne when we just wanna create one.

```js
db.tours.find();
```

> And notice how it also automatically createdthis object ID here which is the unique identifierof this document.So remember how I said that MongoDB would automaticallycreate these unique identifiers behind the scenes,and so that's exactly what you see here.And besides that, well, you see that this reallyis just a **regular JSON object**.And so this makes it great and really easy to work with JavaScript, and so as you can guess, this is going tomake it really easy to work with MongoDB data in JavaScript.Because we're already using **kind of the same format that we're already used to in JavaScript.** so that is one of the main reasons why MongoDB is so popular for Node JS applications. Okay, now another very useful command is show dbs,which will basically show us all the databases that we have in MongoDB, and so here you see our itsyuimoriiTest database.But we also have some other ones,which MongoDB automatically creates for us.And yours might not be the same as these three,but if not, don't worry, and just like beforewe could use the "use" command to switch to one of these.So let's say we now wanted to use admin, and so nowwe switched to DB admin, all right. So again, **"use" is to switch to an existing databaseor to create a new one,** if the name thatwe pass into it does not yet exist.

```js
show collections
```

### CRUD (querying) Reading docuemnts

`db.tours.find({difficulty: "easy"});` `db.tour.find({ price: {$lte:  500 })`

### Create a hosted database with altas

#### create a new project

![image](https://res.cloudinary.com/dxmfrq4tk/image/upload/v1678502523/node.js%20notes/Screen_Shot_2023-03-10_at_8.37.04_PM_tblzc0.png)

#### create username & password

![image](https://res.cloudinary.com/dxmfrq4tk/image/upload/v1678502617/node.js%20notes/Screen_Shot_2023-03-10_at_8.39.56_PM_fks8he.png)

#### copy the password to local file `config.env`

```
NODE_ENV=development
PORT=3000
# USERNAME=itsyuimorii✨
# PASSWORD=123456

DATABASE_PASSWORD = XSCFFlZyw7PE4cxf
```

#### Add data into this database

![image](https://res.cloudinary.com/dxmfrq4tk/image/upload/v1678586181/Screen_Shot_2023-03-11_at_7.39.26_PM_irjbzu.png)

#### Insert data

![image](https://res.cloudinary.com/dxmfrq4tk/image/upload/v1678586181/Screen_Shot_2023-03-11_at_7.55.51_PM_uj8it8.png)

#### check the data just created on Altas

![image](https://res.cloudinary.com/dxmfrq4tk/image/upload/v1678586288/Screen_Shot_2023-03-11_at_7.57.11_PM_wn3s6b.png)

#### allow access from everywhere

![image](https://res.cloudinary.com/dxmfrq4tk/image/upload/v1678502617/node.js%20notes/Screen_Shot_2023-03-10_at_8.39.56_PM_fks8he.png)

#### connect altas with mongoShell

![](https://res.cloudinary.com/dxmfrq4tk/image/upload/v1678590333/Screen_Shot_2023-03-11_at_9.04.47_PM_ynsynf.png)

Connect with mongdb steel![](https://res.cloudinary.com/dxmfrq4tk/image/upload/v1678586614/Screen_Shot_2023-03-11_at_7.46.49_PM_vitbn7.png)

![](https://res.cloudinary.com/dxmfrq4tk/image/upload/v1678586703/Screen_Shot_2023-03-11_at_8.04.28_PM_nglq3h.png)

![](https://res.cloudinary.com/dxmfrq4tk/image/upload/v1678586728/Screen_Shot_2023-03-11_at_8.05.16_PM_bbjzot.png)

## 💛Mongo and Mongoose

![](https://res.cloudinary.com/dxmfrq4tk/image/upload/v1678590333/Screen_Shot_2023-03-11_at_9.04.47_PM_ynsynf.png)

![](https://res.cloudinary.com/dxmfrq4tk/image/upload/v1678590333/Screen_Shot_2023-03-11_at_9.04.51_PM_szxtt5.png)

![](https://res.cloudinary.com/dxmfrq4tk/image/upload/v1678590511/Screen_Shot_2023-03-11_at_9.07.59_PM_mo11ll.png)

#### Configuration `config.env`

> Config.env

```js
NODE_ENV=development
PORT=3000
# USERNAME=itsyuimorii✨
# PASSWORD=123456

DATABASE=mongodb+srv://itsyuimorii:<PASSWORD>@cluster0.pto1wr6.mongodb.net/itsyuimoriiTours?retryWrites=true&w=majority

DATABASE_PASSWORD = XSCFFlZyw7PE4cxf
```

> ⁉️Be sure to add `database name` `itsyuimoriiTours` here after `/`

#### install mongoose

```js
npm i mongoose@5.5.2
```

> server.js

```js
const mongoose = require("mongoose");

const DB = process.env.DATABASE.replace(
  "<PASSWORD>",
  process.env.DATABASE_PASSWORD
);

mongoose
  .connect(DB, {
    useNewUrlParser: true,
    useUnifiedTopology: true,
  })
  .then(() => console.log("DB connection successful!"));
```

```js
mongoose
  .connect(DB, {
    useNewUrlParser: true,
    useUnifiedTopology: true,
  })
  .then((con) => {
    console.log("DB connection successful!");
    console.log(con.connection);
  });

// console.log(con.connection); can get the details of DB connection info
```

#### Mongoose theroy

👉 Mongoose is an Object Data Modeling (ODM) library forMongoDB and Node.js, a higher level of abstraction;

👉 Mongoose allows for rapid and simple development ofmongoDB database interactions;

👉 Features: schemas to model data and relationships, easydata validation, simple query API, middleware, etc;

👉 Mongoose schema: where we model our data, by describingthe structure of the data, default values, and validation;

👉 Mongoose model: a wrapper for the schema, providing aninterface to the database for CRUD operations.

#### [Mongoose API](https://mongoosejs.com/docs/api/mongoose.html)

#### Create schema

```js
const mongoose = require("mongoose");

const tourSchema = new mongoose.Schema({
  name: {
    type: String,
    required: [true, `A tour must have a name`],
    unique: true,
  },
  rating: {
    type: String,
    default: 4.5,
  },
  price: {
    type: Number,
    required: [true, `A tour must have a price`],
  },
});
```

#### Creating documents and testing the model

```js
const Tour = mongoose.model("Tour", tourSchema);

//this code only can run one, the second time will be 11000 error💥
const testTour = new Tour({
  name: "The Forest Hiker",
  rating: 4.5,
  price: 497,
});

testTour
  .save()
  .then((doc) => {
    console.log(doc);
  })
  .catch((err) => console.log("ERROR💥:", err));
```

> npm start , add new data to db
>
> ```bash
> {
>   rating: '4.5',
>   _id: 640d57ccf040722ada917d32,
>   name: 'The Forest Hiker',
>   price: 497,
>   __v: 0
> }
> ```
>
> ```js
> const testTour = new Tour({
>   name: "The Park Camper ",
> });
> //error💥: _message: 'Tour validation failed'
> ```

## 💛 Backend architecture - MVC

![](https://res.cloudinary.com/dxmfrq4tk/image/upload/v1678596837/Screen_Shot_2023-03-11_at_10.53.38_PM_frtc08.png)

In this architecture,the model layer is concerned with everything about applications data,and the business logic.

we have the controller layer and the function of the controllers is to handle the application's request,interact with models,and send back responses to the client.And all that is called the `application logic`.

Finally, the view layer is necessary if we have a graphical interface in our app.Or in other words, if we're building a server-side rendered website,as we talked about before.In this case, the **view layer** consists basically of the templates used to generate the view,so the website that we're going to send back to the client.And that is the presentation logic.

For now, we're just building an API though,so we're not really concerned about views just yet.

That's for a bit later in the course.So using a pattern, or an architecture like thisallows us to write a more modular application, which is going to be way easier to maintain in scale,as necessary.And we could take it even further,and add more layers of abstraction here.But in this kind of smaller application,the MVC architecture is more than enough for our needs.

Now, all this may sound a bit abstract,so let's take a look at MVC in the context of our app,and the request-response cycle.So as always, it all starts with a request.That request will hit one of our routers,because remember, we have multiple routers.Basically, one for each resource,like tours, users, et cetera.Now the goal of the router is to delegate the requestto the correct handler function,which will be in one of the controllers.

And again, there will be one controllerfor each of our resources,to keep these different parts of the app nicely separated.Then, depending on the incoming request,the controller might need to interactwith one of the models,for example to retrievea certain document from the database,or to create a new one.Once more, there is one model file for each resource.

After getting the data from the model,the controller might then be ready to send back a responseto the client, for example, containing that data.Now, in case we want to actually render a website,there is one more step involved.In this case, after getting the data from the model,the controller will then selectone of the view templates and inject the data into it.That rendered website will then be sent backas the response.In the view layer in an Express appthere is usually one view template for each page.Like a tour overview page,a tour detail page, or a login page.In the example of our latest app of course.So, that is a broad overview of the architecturethat we're going to implement in this project.

![M](https://res.cloudinary.com/dxmfrq4tk/image/upload/v1678598030/Screen_Shot_2023-03-11_at_11.13.30_PM_ezujwc.png)

Now to finish, let me just go into a bit more detail on model and controller. So, one of the big `goals of MVC` is to **separate business logic from application logic.**

**Application logic** is all the code that is only concerned about the `application's implementation` and not the **underlying business problem** that we're actually ` trying to solve with the application.` _Like showing and selling tours,managing stock in a supermarket,or organizing a library_ .So again, **application logicis the logic that makes the app actually work**. For example, *a big part of application logic in Express,is all about managing requests and responses.*So, in a sense, we can also say that `application logic is more about technical stuff. `

Also, if we have views in our app,the **application logic serves as a bridge between model and view layers** So that we `never mix business logic with presentation logic`.

**Business logic**,it's all the code that actually solves the business problem that we set out to solve. Let's say again, that our goal is to show tours to customers and then sell them. And the code that is directly related to the business rules,to how the business works,and the business needs, is business logic.Now if that still sounds a bit too philosophical,some examples in the context of our latest appare creating new tours in the app's database,checking if a user's password is correct when he logs in,validating user input data,or ensuring that only users who bought a certain tourcan review it.So all this stuff is concerned with the business itself,and so it's part of the business logic.

Now, we need to keep in mind that application logic and business logic are almost impossible to completely separate,and so sometimes they will overlap.But we should do our best effortsto keep the application logic in our controllers and business logic in our models.And there is even this philosophy offat models, thin controllers,which says we should offload as much logic as possible into the models,to keep the controllers as simple and lean as possible.So a fat model will have as much business logicas we can offload to it,and a thin controller will have as little logic as possible,so that the **controller is really mostly for managing the application's requests and responses.**



# 🚧Using MongoDB with Mongoose

### Refactoring MVC

> Routes/tourRoutes.js

```js
const express = require("express");

const tourController = require("../controllers/tourController");

const router = express.Router();

/* use this .param function here to define parameter middleware in your own applications  */
// router.param('id', tourController.checkID);

router
  .route("/")
  .get(tourController.getAllTours)
  .post(tourController.checkBody, tourController.createTour);
router
  .route("/:id")
  .get(tourController.getTour)
  .patch(tourController.createTour)
  .delete(tourController.deleteTour);

module.exports = router;
```

> controllers/tourController.js

```js
const Tour = require("../models/tourModel");

exports.checkBody = (req, res, next) => {
  if (!req.body.name || !req.body.price) {
    return res.status(400).json({
      status: "fail",
      message: "Missing name or price",
    });
  }
  next();
};

// 2) ROUTE HANDLER
exports.getAllTours = (req, res) => {
  console.log(req.requestTime);

  res.status(200).json({
    status: "success",
    requestedAt: req.requestTime,
  });
};

exports.getTour = (req, res) => {
  console.log(req.params);
  const id = req.params.id * 1;
};

exports.createTour = (req, res) => {
  res.status(201).json({
    status: "success",
  });
};

exports.updateTour = (req, res) => {
  res.status(200).json({
    status: "success",
  });
};

exports.deleteTour = (req, res) => {
  res.status(204).json({
    status: "success",
    data: null,
  });
};
```

### Creating Documents

```js
exports.createTour = async (req, res) => {
  // const newTour = new Tour({});
  // newTour.save();
  try {
    //Async function, return a Promise
    const newTour = await Tour.create(req.body);

    res.status(201).json({
      status: "success",
      data: {
        tour: newTour,
      },
    });
  } catch (err) {
    res.status(400).json({
      status: `fail`,
    });
  }
};
```

![](https://res.cloudinary.com/dxmfrq4tk/image/upload/v1678638073/Screen_Shot_2023-03-12_at_11.14.32_AM_jfcip8.png)

### Reading Documents with Mongoose

> in order to implement or get tour and get all tour's route handlers.

```js
exports.getAllTours = async (req, res) => {
  //return all the documents in this collection
  try {
    //query for all the documents,using find() method, it will return an array of all these documents,and will also very nicely convert them into JavaScript objects

    const getAllTours = await Tour.find();
    res.status(200).json({
      status: "success",
      //result measures the number of results that are in the tours
      result: getAllTours.length,
      //this data property here to envelope the tours.
      data: {
        tours: getAllTours,
      },
    });
  } catch (err) {
    res.status(404).json({
      status: "fail",
      message: err,
    });
  }
};
```

![](https://res.cloudinary.com/dxmfrq4tk/image/upload/v1678640381/Screen_Shot_2023-03-12_at_11.58.57_AM_awsqqc.png)

### GetTour with Mongoose

```js
exports.getTour = async (req, res) => {
  try {
    const getTour = await Tour.findById(req.params.id);
    //Tour.findOne({_id: req.params.id})
    res.status(200).json({
      status: "success",
      data: {
        getTour,
      },
    });
  } catch (err) {
    res.status(404).json({
      status: `fail`,
      message: err,
    });
  }
};
```

![](https://res.cloudinary.com/dxmfrq4tk/image/upload/v1678640883/Screen_Shot_2023-03-12_at_12.07.12_PM_w969zt.png)

![](https://res.cloudinary.com/dxmfrq4tk/image/upload/v1678641164/Screen_Shot_2023-03-12_at_12.11.29_PM_vnqouf.png)

### UpdateTour with Mongoose

```js
exports.deleteTour = async (req, res) => {
  try {
    //query the documents
    //So in this case, we actually don't save anything to any variable because remember, we actually don't send anything back to the client
    await Tour.findByIdAndDelete(req.params.id);
    res.status(204).json({
      status: "success",
      data: null,
    });
  } catch (err) {
    res.status(400).json({
      status: `fail`,
      message: err,
    });
  }
};
```

![](https://res.cloudinary.com/dxmfrq4tk/image/upload/v1678642843/Screen_Shot_2023-03-12_at_12.40.21_PM_msjhtn.png)

Reference https://www.natours.dev/api/v1/tours

## 💛 Importing Development Data

> create a script import JSON into dababase `dev-data/data/import-dev-data.js`
>
> ```js
> const fs = require("fs");
> const mongoose = require("mongoose");
> const dotenv = require("dotenv");
> const Tour = require("../../models/tourModel");
>
> dotenv.config({ path: "./config.env" });
>
> const DB = process.env.DATABASE.replace(
>   "<PASSWORD>",
>   process.env.DATABASE_PASSWORD
> );
>
> mongoose
>   .connect(DB, {
>     useNewUrlParser: true,
>     useUnifiedTopology: true,
>   })
>   .then((con) => {
>     console.log("DB connection successful!");
>     // console.log(con.connection);
>   });
>
> //READ JSON FILE
> //parse json file into javascript object
> const tours = JSON.parse(
>   fs.readFileSync(`${__dirname}/tours-simple.json`, "utf-8")
> );
>
> //IMPORT JSON DATA INTO DB
> const importData = async () => {
>   try {
>     await Tour.create(tours);
>     console.log("Data successfully created");
>   } catch (error) {
>     console.log(error);
>   }
> };
>
> //DELETE ALL DATA FROM COLLECTION
> const deleteData = async () => {
>   try {
>     await Tour.deleteMany();
>     console.log("Data successfully deleted");
>     process.exit();
>   } catch (error) {
>     console.log(error);
>   }
> };
>
> if (process.argv[2] === "--import") {
>   importData();
> } else if (process.argv[2] === "--delete") {
>   deleteData();
> }
> ```
>
> ```bash
> node dev-data/data/import-dev-data.js --import
> node dev-data/data/import-dev-data.js --delete
> ```

### Making an API better: Filtering

💥get the log from console 

> server.js

```js
const dotenv = require('dotenv');
dotenv.config({ path: './config.env' });
```

```js
postman: 127.0.0.1:3000/api/v1/tours?duration=5&difficulty=easy
```

```js
exports.getAllTours = async (req, res) => {
  //return all the documents in this collection
  try {
    console.log(req.query);
    ...
```

then we could get the  `duration=5&difficulty=easy` in console

💥In mongo ,two ways to write query  

```js 
    //first way to writing query( currently using)
    const getAllTours = await Tour.find({
      duration: 5,
      difficulty: 'easy',
    });

    //second way to writing query
    const getAllTours = await Tour.find()
      .where('duration')
      .equal(5)
      .where('difficulty')
      .equal('easy');
```

![](https://res.cloudinary.com/dxmfrq4tk/image/upload/v1678995080/Screen_Shot_2023-03-16_at_2.30.39_PM_dmytpe.png)

```js
// 2) ROUTE HANDLER
exports.getAllTours = async (req, res) => {
  //return all the documents in this collection
  try {
    //query for all the documents,using find() method, it will return an array of all these documents,and will also very nicely convert them into JavaScript objects

    //BUILD QUERY
    /*
    Here we need, really, a hard copy.We can't just do, request dot query,because then, if you would delete somethingfrom this object, we would also delete itfrom the req dot query object.And that's because in JavaScript,when we set a variable to another object,that new variable will basically just bea reference to that original object.So we really need a hard copy here.In JavaScript, there's not really a built-in wayof doing this, but a very nice trick that we can use,since ES6, is to use, first, the structuringwhich we use by using dot dot dot.And then we can simply create anew object out of that.Just like this.This structuring here, these three dots,will basically take all the fields out of the object.Here with the curly braces, well,we simply create a new object.So we have a new object that is basicallygoing to contain all the key value pairsthat were in our req dot query object.So that's a copy.*/    
    const queryObj = { ...req.query };
    
    
    // Now let's create an array ofall the fields that we want to exclude.I'm calling that one excluded fields.And then we want to exclude page,so that's the one that I justtold you before in Postman.Then, we also want to excludesort,we want to exclude limit,and we want to exclude fields.We will implement all of the functionalityof paging, sorting, limiting, and selectingonly some specific fields, laterover the next couple of videos. 
    const excludeFileds = ['page', 'sort', 'limit', 'fields'];
    
    //Next, what we need to do is tobasically remove all of these fields from our query object.
    excludeFileds.forEach((el) => delete queryObj[el]);
    
		//testing the method words, req.query should be all items, and queryObj is after excluding
    console.log(req.query, queryObj, excludeFileds);
    
    //get query without excluding fields
    const query = await Tour.find(queryObj);

    //EXECUTE QUERY
    const getAllTours = await query;

    
    res.status(200).json({
      status: 'success',
      //result measures the number of results that are in the tours
      result: getAllTours.length,
      //this data property here to envelope the tours.
      data: {
        tours: getAllTours,
      },
    });
  } catch (err) {
    res.status(404).json({
      status: 'fail',
      message: err,
    });
  }
};
```

- Tour.find(queryObj) will return a `query`

### Making an API better: Advanced Filtering

Query operators

```js
use itsyuimoriiTours
//$:mongo operator
db.tours.find({ price: {$lte: 500} })   

db.tours.find({ $or: [ {price: {$lt: 500}}, {rating: {$gte: 4.8}}  ] })
```



- Using advance filter such as greater than, less than.... to filter querying, 

  > tourController

```js
const Tour = require('../models/tourModel');

// 2) ROUTE HANDLER
exports.getAllTours = async (req, res) => {
  //return all the documents in this collection
  try {
    //console.log(req.query);
    //query for all the documents,using find() method, it will return an array of all these documents,and will also very nicely convert them into JavaScript objects

    //BUILD QUERY
    // 1)Filtering
    const queryObj = { ...req.query };
    const excludeFileds = ['page', 'sort', 'limit', 'fields'];
    excludeFileds.forEach((el) => delete queryObj[el]);
    // console.log(req.query, queryObj, excludeFileds);

    // 2) Advanced filtering

    let queryStr = JSON.stringify(queryObj);
    //using regex expressions
    queryStr = queryStr.replace(/\b(gte|gt|lte|lt)\b/g, (match) => `$${match}`);
    console.log(JSON.parse(queryStr));

    const query = Tour.find(JSON.parse(queryStr));
    //EXECUTE QUERY
    const getAllTours = await query;

    /*  //second way to writing query

    const getAllTours = await Tour.find()
      .where('duration')
      .equal(5)
      .where('difficulty')
      .equal('easy'); */

    res.status(200).json({
      status: 'success',
      //result measures the number of results that are in the tours
      result: getAllTours.length,
      //this data property here to envelope the tours.
      data: {
        tours: getAllTours,
      },
    });
  } catch (err) {
    res.status(404).json({
      status: 'fail',
      message: err,
    });
  }
};
```

> Test: `127.0.0.1:3000/api/v1/tours?duration[gte]=5&difficulty=easy`

![](https://res.cloudinary.com/dxmfrq4tk/image/upload/v1678728132/Screen_Shot_2023-03-13_at_12.20.31_PM_gdxuzp.png)

### Making the API better: Sorting

- Sorting by price

```js
    // 2) Sorting
    if (req.query.sort) {
      const sortBy = req.query.sort.split(',').join(' ');
      // console.log(sortBy);
      query = query.sort(sortBy);
      //So in case that the user does not specify any sort field in the URL query string, we're still gonna add a sort to the query. So query.sort and we will then sort by the created add field, all right? And actually in a descending order, so that the newest ones appear first. So minus created at.
    } else {
      query = query.sort('-createdAt');
    }
```

```js
127.0.0.1:3000/api/v1/tours?sort=-price 
```

### Making the API better: Limiting fields


to choose which fields they wantto get back in the response .So, for a client, **it's always ideal to receive as little data as possible,in order to reduce the band width that is consumed with each request.** And that's, of course, especially true when we have really data-heavy data sets, right? And so it's a very nice feature to allow the API user to only request some of the fields.

So, as the third feature, we will have field limiting.And, just like before, let me start by showing you  how it's gonna work here in Postman.So, we're gonna specify a field called fields,and then the name of the fields that we actually want to receive.So let's say we only want **the name,the duration,the difficulty, and the price,** And so, the implementation will actually be very similar to what we did before with sorting.

```js
  // 3) Field Limiting
    if (req.query.fields) {
      //get field from postman
      const fields = req.query.fields.split(',s').join(' ');
      query = query.select(fields);
    } else {
      //excluding this field
      // query = query.select('-__v');
    }

```

> Postman `127.0.0.1:3000/api/v1/tours?fields=name,duration,difficulty,price`

![](https://res.cloudinary.com/dxmfrq4tk/image/upload/v1678939417/Screen_Shot_2023-03-15_at_11.02.59_PM_svkxal.png)

### Making the API better: Pagination

And now what kind of methods do we have in order to implement pagination? The most useful one is **skip**and so this limit here is actually exactly the same as the limit that we defined in the query string. So basically the amount of results that we want in the query. And then **skip**, that we have here, *is the amount of results that should be skipped before actually querying data. bAll right, so again, this is just a placeholder here.*

```js
  // 4) Pagination
    //convert string into number by "*1"
    const page = req.query.page * 1 || 1;
    const limit = req.query.limit * 1 || 100;
    //define the skip value
    const skip = (page - 1) * limit;
    // page=3&limit=10 page1(1-10)  page2(11-20)  page3(21-30 )
    query = query.skip(skip).limit(limit);

    //for the situations that page=4&limit=3, not enough 4 pages that show 3 results, using 👇
    if (req.query.page) {
      const numTours = await Tour.countDocuments();
      if (skip > numTours) throw new Error('this page does not exist');
    }
    //EXECUTE QUERY
    const getAllTours = await query
```

![](https://res.cloudinary.com/dxmfrq4tk/image/upload/v1678939732/Screen_Shot_2023-03-15_at_11.08.27_PM_zoxujg.png)

### Making the API Better: Aliasing

we can add to an API is to **provide an alias route to a request** that might be very popular, so it might be requested all the time. Example: top-5-cheap routes

```js
router.route('/top-5-cheap').get(tourController.getAllTours);
```

```js
//get five cheapest routes in postman
127.0.0.1:3000/api/v1/tours?limit=5&sort=-ratingAverage,price
```

we basically want to **prefill some of the fields** in the query string.So, we already knowthat our query string should look like this.And, actually we can add some more stuff here.But, in essence, our query string has to look something like this.And so, the solution is gonna be to **run a middleware** before we actually run this ` handler`.And so, that middleware function is then gonna **manipulate the query object** that's coming in.And so, this is yet another really nice example of using middleware because you really needto get familiar and used to this conceptof **using middleware strategically in orderto change the request object** as we need it.

In order to make our query string like this `router.route('/top-5-cheap').get(tourController.getAllTours);`, we need too write a middleware function called `aliasTopTours`

> controllers/tourController.js

```js
exports.aliasTopTours = (req, res, next) => {
  req.query.limit = '5';
  req.query.sort = '-ratingAverage,price';
  req.query.fields = 'name,price,ratingAverage,summary,difficulty';
  next();
};
```

> Routes/tourRoutes.js

```js
router
  .route('/top-5-cheap')
  .get(tourController.aliasTopTours, tourController.getAllTours);
```

Testing: 👇

 ![](https://res.cloudinary.com/dxmfrq4tk/image/upload/v1678991812/Screen_Shot_2023-03-16_at_1.36.39_PM_x2j5to.png)

### ALL code before refactoring API features

```js
const Tour = require('../models/tourModel');
//alias middleware
exports.aliasTopTours = (req, res, next) => {
  req.query.limit = '5';
  req.query.sort = '-ratingAverage,price';
  req.query.fields = 'name,price,ratingAverage,summary,difficulty';
  next();
};

// 2) ROUTE HANDLER
exports.getAllTours = async (req, res) => {
  //return all the documents in this collection
  try {
    //console.log(req.query);
    //query for all the documents,using find() method, it will return an array of all these documents,and will also very nicely convert them into JavaScript objects

    //BUILD QUERY
    // 1A) Filtering
    const queryObj = { ...req.query };
    const excludeFields = ['page', 'sort', 'limit', 'fields'];
    excludeFields.forEach((el) => delete queryObj[el]);
    // console.log(req.query, queryObj, excludeFields);
    // const query = Tour.find(queryObj);

    // 1B) Advanced filtering

    let queryStr = JSON.stringify(queryObj);
    //using regex expressions
    queryStr = queryStr.replace(/\b(gte|gt|lte|lt)\b/g, (match) => `$${match}`);
    // console.log(JSON.parse(queryStr));

    let query = Tour.find(JSON.parse(queryStr));

    // 2) Sorting
    if (req.query.sort) {
      const sortBy = req.query.sort.split(',').join(' ');
      // console.log(sortBy);
      query = query.sort(sortBy);
    } else {
      query = query.sort('-createdAt');
    }

    // 3) Field Limiting
    if (req.query.fields) {
      //get field from postman
      const fields = req.query.fields.split(',').join(' ');
      query = query.select(fields);
    } else {
      //excluding this field
      query = query.select('-__v');
    }

    // 4) Pagination
    //convert string into number by "*1"
    const page = req.query.page * 1 || 1;
    const limit = req.query.limit * 1 || 100;
    //define the skip value
    const skip = (page - 1) * limit;
    // page=3&limit=10 page1(1-10)  page2(11-20)  page3(21-30 )
    query = query.skip(skip).limit(limit);

    //for the situations that page=4&limit=3, not enough 4 pages that show 3 results, using 👇
    if (req.query.page) {
      const numTours = await Tour.countDocuments();
      if (skip > numTours) throw new Error('this page does not exist');
    }
    //EXECUTE QUERY
    const getAllTours = await query;

    /*  //second way to writing query

    const getAllTours = await Tour.find()
      .where('duration')
      .equal(5)
      .where('difficulty')
      .equal('easy'); */

    res.status(200).json({
      status: 'success',
      //result measures the number of results that are in the tours
      result: getAllTours.length,
      //this data property here to envelope the tours.
      data: {
        tours: getAllTours,
      },
    });
  } catch (err) {
    res.status(404).json({
      status: 'fail',
      message: err,
    });
  }
};

exports.getTour = async (req, res) => {
  try {
    const getTour = await Tour.findById(req.params.id);
    //Tour.findOne({_id: req.params.id})
    res.status(200).json({
      status: 'success',
      data: {
        getTour,
      },
    });
  } catch (err) {
    res.status(404).json({
      status: `fail`,
      message: err,
    });
  }
};

//create document with mongoose
exports.createTour = async (req, res) => {
  // const newTour = new Tour({});
  // newTour.save();
  try {
    //Async function, return a Promise
    const newTour = await Tour.create(req.body);

    res.status(201).json({
      status: 'success',
      data: {
        tour: newTour,
      },
    });
  } catch (err) {
    res.status(400).json({
      status: `fail`,
      message: err,
    });
  }
};

exports.updateTour = async (req, res) => {
  try {
    //query the documents
    const updateTour = await Tour.findByIdAndUpdate(req.params.id, req.body, {
      new: true,
      runValidator: true,
    });
    res.status(200).json({
      status: 'success',
    });
  } catch (err) {
    res.status(400).json({
      status: `fail`,
      message: err,
    });
  }
};

exports.deleteTour = async (req, res) => {
  try {
    //query the documents
    //So in this case, we actually don't save anything to any variable because remember, we actually don't send anything back to the client
    await Tour.findByIdAndDelete(req.params.id);
    res.status(204).json({
      status: 'success',
      data: null,
    });
  } catch (err) {
    res.status(400).json({
      status: `fail`,
      message: err,
    });
  }
};
```

## 💛Refactoring API features

> controllers/tourController.js

```js
const Tour = require('../models/tourModel');

const APIFeatures = require('../utils/apiFeatures');

//alias middleware
exports.aliasTopTours = (req, res, next) => {
  req.query.limit = '5';
  req.query.sort = '-ratingsAverage,price';
  req.query.fields = 'name,price,ratingsAverage,summary,difficulty';
  next();
};

// 2) ROUTE HANDLER
exports.getAllTours = async (req, res) => {
  try {
    // EXECUTE QUERY
    const features = new APIFeatures(Tour.find(), req.query)
      .filter()
      .sort()
      .limitFields()
      .paginate();
    const tours = await features.query;

    // SEND RESPONSE
    res.status(200).json({
      status: 'success',
      results: tours.length,
      data: {
        tours,
      },
    });
  } catch (err) {
    res.status(404).json({
      status: 'fail',
      message: err,
    });
  }
};

exports.getTour = async (req, res) => {
  try {
    const tour = await Tour.findById(req.params.id);
    // Tour.findOne({ _id: req.params.id })

    res.status(200).json({
      status: 'success',
      data: {
        tour,
      },
    });
  } catch (err) {
    res.status(404).json({
      status: 'fail',
      message: err,
    });
  }
};

exports.createTour = async (req, res) => {
  try {
    // const newTour = new Tour({})
    // newTour.save()

    const newTour = await Tour.create(req.body);

    res.status(201).json({
      status: 'success',
      data: {
        tour: newTour,
      },
    });
  } catch (err) {
    res.status(400).json({
      status: 'fail',
      message: err,
    });
  }
};

exports.updateTour = async (req, res) => {
  try {
    const tour = await Tour.findByIdAndUpdate(req.params.id, req.body, {
      new: true,
      runValidators: true,
    });

    res.status(200).json({
      status: 'success',
      data: {
        tour,
      },
    });
  } catch (err) {
    res.status(404).json({
      status: 'fail',
      message: err,
    });
  }
};
//no info send back to the client
exports.deleteTour = async (req, res) => {
  try {
    await Tour.findByIdAndDelete(req.params.id);

    res.status(204).json({
      status: 'success',
      data: null,
    });
  } catch (err) {
    res.status(404).json({
      status: 'fail',
      message: err,
    });
  }
};

```

> utils/apiFeatures.js

```js
class APIFeatures {
  // two variables => the mongoose query and also the queryString from express
  //Now, again, I'm passing the query here because I do not want to query inside of this class because that would then bounce this class to the tour resource but, again,I want this to be as reusable as possible.
  constructor(query, queryString) {
    this.query = query;
    this.queryString = queryString;
  }

  filter() {
    const queryObj = { ...this.queryString };
    const excludedFields = ['page', 'sort', 'limit', 'fields'];
    excludedFields.forEach((el) => delete queryObj[el]);

    // 1B) Advanced filtering
    let queryStr = JSON.stringify(queryObj);
    queryStr = queryStr.replace(/\b(gte|gt|lte|lt)\b/g, (match) => `$${match}`);

    this.query = this.query.find(JSON.parse(queryStr));

    return this;
  }

  sort() {
    if (this.queryString.sort) {
      const sortBy = this.queryString.sort.split(',').join(' ');
      this.query = this.query.sort(sortBy);
    } else {
      this.query = this.query.sort('-createdAt');
    }

    return this;
  }

  limitFields() {
    if (this.queryString.fields) {
      const fields = this.queryString.fields.split(',').join(' ');
      this.query = this.query.select(fields);
    } else {
      this.query = this.query.select('-__v');
    }

    return this;
  }

  paginate() {
    const page = this.queryString.page * 1 || 1;
    const limit = this.queryString.limit * 1 || 100;
    const skip = (page - 1) * limit;

    this.query = this.query.skip(skip).limit(limit);

    return this;
  }
}
module.exports = APIFeatures;

```

## 💛Aggregation Pipeline: Matching and Grouping

[Aggregation Operations reference]![img](https://www.mongodb.com/docs/manual/assets/link.svg)(https://www.mongodb.com/docs/manual/aggregation/) 

MongoDB aggregation pipeline which is an extremely powerful and extremely useful MongoDB framework for data aggregation. And the idea is that **we basically define a pipeline that all documents from a certain collection go through where they are processed step by step in order to transform them into aggregated results.** 

For example, we can use the aggregation pipeline in order to calculate averages or calculating minimum and maximum values or we can calculate distances even, and we can really do all kinds of stuff. It's really amazing how powerful this aggregation pipeline is.



So I want to create a function here that's gonna calculate a couple of statistics about our tours. So I'm gonna call this one getTourStats.

```js
//💛Aggregation Pipeline: Matching and Grouping
exports.getTourStats = async (req, res) => {
  try {
    const stats = await Tour.aggregate([
    ]);

    res.status(200).json({
      status: 'success',
      data: {
        stats,
      },
    });
  } catch (err) {
    res.status(404).json({
      status: 'fail',
      message: err,
    });
  }
};

```

Now the aggregation pipeline really is a MongoDB feature.But **Mongoose, of course, gives us access to it,so that we can use it in the Mongoose driver,** right? So using our tour model in order to accessthe tour collection, we say **tour.aggregate.**

And so the aggregation pipeline is a bitlike a regular query and so using the aggregation pipelineit's a just a bit like doing a **regular query.**

The difference here is that in aggregations, **we can manipulate the data in a couple of different steps and so let's now actually define these steps.**And for that, we pass in an array of so-called **stages**. So we pass in an array, and then here we will then have a lot of stages.And again the documents then pass through these stagesone by one, step by step in the define sequenceas we define it here. So each of the elements in this array will be one of the stages.

> /controllers/tourControllers

```js
exports.getTourStats = async (req, res) => {
  try {
    const stats = await Tour.aggregate([
      {
        $match: { ratingAverage: { $gte: 4.5 } },
      },
      {
        $group: {
          _id: { $toUpper: '$difficulty' },
          numTours: { $sum: 1 },
          numRatings: { $sum: '$ratingQuantity' },
          avgRating: { $avg: '$ratingAverage' },
          avgPrice: { $avg: '$price' },
          minPrice: { $min: '$price' },
          maxPrice: { $max: '$price' },
        },
      },
      {
        $sort: { avgPrice: 1 },
      },
      // {
      //   $match: { _id: { $ne: 'EASY' } }
      // }
    ]);

    res.status(200).json({
      status: 'success',
      data: {
        stats,
      },
    });
  } catch (err) {
    res.status(404).json({
      status: 'fail',
      message: err,
    });
  }
};
```

> Routes/tourRoutes.js

```js
router.route('/tour-stats').get(tourController.getTourStats);
```

## Aggregation Pipeline: Unwinding and Projecting

```js
exports.getMonthlyPlan = async (req, res) => {
  try {
    const year = req.params.year * 1; // 2021

    const plan = await Tour.aggregate([
      {
        $unwind: '$startDates',
      },
      {
        $match: {
          startDates: {
            $gte: new Date(`${year}-01-01`),
            $lte: new Date(`${year}-12-31`),
          },
        },
      },
      {
        $group: {
          _id: { $month: '$startDates' },
          numTourStarts: { $sum: 1 },
          tours: { $push: '$name' },
        },
      },
      {
        $addFields: { month: '$_id' },
      },
      {
        $project: {
          _id: 0,
        },
      },
      {
        $sort: { numTourStarts: -1 },
      },
      {
        $limit: 12,
      },
    ]);

    res.status(200).json({
      status: 'success',
      data: {
        plan,
      },
    });
  } catch (err) {
    res.status(404).json({
      status: 'fail',
      message: err,
    });
  }
};
```

> Routes/tourRoutes.js

```js
router.route('/monthly-plan/:year').get(tourController.getMonthlyPlan);
```

![](https://res.cloudinary.com/dxmfrq4tk/image/upload/v1679243151/node.js notes/Screen_Shot_2023-03-19_at_11.25.45_AM_ocwmcm.png)

```js
 const plan = await Tour.aggregate([
      {
        //basically deconstruct an array field from the info documents and then output one document for each element of the array.
        $unwind: '$startDates',
      },
      {
        $match: {
          startDates: {
            $gte: new Date(`${year}-01-01`),
            $lte: new Date(`${year}-12-31`),
          },
        },
      },
      {
        $group: {
          //group it by month
          _id: { $month: '$startDates' },
          numTourStarts: { $sum: 1 },
          tours: { $push: '$name' },
        },
      },
    ]);
```

![](https://res.cloudinary.com/dxmfrq4tk/image/upload/v1679272909/node.js notes/Screen_Shot_2023-03-19_at_7.41.24_PM_v0ii82.png)

## Virtual Properties

Define the virtual property

now virtual properties are basically fieldsthat we can define on our schemabut that will not be persisted.So they will not be saved into the databasein order to save us some space there.And most of the time, of course,we want to really save our data to the database,but virtual properties make a lot of sens efor fields that can be derived from one another.For example a conversion from miles to kilometers,it doesn't make sense to store these two fieldsin a database if we can easily convertone to the other, right?

And that's because we need to explicitly definein our schema that we want the virtual propertiesin our output.And so remember how I said that into this **Mongoose.schema,we can pass in not only the objectwith the schema definition itself,but also an object for the schema options.**And so let's add that here at the end,so this first object here is the schema definition,and now second **an object for the options.**And what we need to specify here is the `to JSON  `  property here, and what we say is then that **each time that the data is actually outputted as JSON,we want virtuals to be true**.So basically the virtuals to be part of the output.And now I'm duplicating this because we also want to say to object.

> models/tourModel.js

```js

const tourSchema = new mongoose.Schema(
 
.....
  {
    toJSON: { virtuals: true },
    toOBJECT: { virtuals: true },
  }
);

//use regular functions is becuase we want to get 'this', arrow function doesnt have 'this'
tourSchema.virtual('durationWeeks').get(function () {
  return this.duration / 7;
});
```

 ![](https://res.cloudinary.com/dxmfrq4tk/image/upload/v1679275946/node.js notes/Screen_Shot_2023-03-19_at_8.31.52_PM_gab5u3.png)

Now one thing that we need to keep in mindis that we cannot use this virtual property herein a query, because they're technically not part of the database.So we can not say, for example,tour.find where duration weeks is equal to one.That's not gonna work, again because this propertyis not actually part of the database.Now of course we could also have done this conversioneach time after we query the data, for example,like in a controller, but that would notbe the best practice simply because we want to tryto keep business logic and application logic as much separated as possible, remember? So that was that whole talk about fat modelsand thin controllers that we talked about beforewhich says that we should have modelswith as much business logic as we can off load to themand thin controllers with as littlebusiness logic as possible.And so virtual properties like this are actually a good example of how we can achievethat kind of architecture.So knowing the duration in weeksis a business logic because it has to dowith the business itself, not with stuff like requestsor responses, and so we do the calculationright in the model where it belongsand not in the controller.

## Document middleware(Mongoose)

Now, just like with Express, we can use **Mongoose middleware** to make **something happen between two event**s. For example, each time a new document is saved to the database, we can **run a function between the save command is issued and the actual saving of the document,** or also after the actual saving.And that's the reason why Mongoose middlewareis also called **pre and post hooks.  **

So there are four types of middleware in Mongoose: **document, query, aggregate, and model middleware.**And in this lecture, we're gonna talk about **document middleware,** **which is middleware that can act on the currently processed document.**

So just like the virtual properties,we define a middleware on the schema,so `tourSchema.pre`.And so this is for **pre middleware,** which again,is gonna **run before an actual event.**And that event in this case is the **save** event. And so this callback function that we're gonna define here next, so  this function will be called before an **actual documentis saved to the database.** So this is document middleware,and it runs, let me write all of that here. So it runs before the **.save()** command and the **.create()** command. But not on insert many. 

So if we use this command here, so insertMany,then that will actually not trigger the save middleware.So always keep that in mind.It's very important to realize that only on saveand on create actually this middleware hereis gonna be executed.And so let's start by doing a simple console.logand what I'm gonna log through the console is this.And so in a save middleware, the disk keyword hereis gonna point to the currently processed document.And that is the reason why it is called document middleware.

Again, because in this function here,we have access to the document that is being processed. So in this case, the document that is being saved.And so let me just very quickly show that to you using this console.log.And so in order to now trigger this function,remember we need to run a save command or a create command.And so we now need to create a new tour using our API in order to, yeah, to then trigger this middleware.So let's go here to, to postman, and I'm gonna start byby saving these two new routes that we created 

```js
//DOCUMENT MIDDLEWARE : runs before .save() and .create() methods
tourSchema.pre('save', function () {
  console.log(this);
});
```

and so this is what our document is looking like right before it saved into the database.

![](https://res.cloudinary.com/dxmfrq4tk/image/upload/v1679277198/node.js notes/Screen_Shot_2023-03-19_at_8.50.38_PM_df0pmd.png)

And **so at this point of time, we can still act on the data before it is then saved to the database and that is exactly** what I wanna do here is to create a slug for each of these documents. So remember how in the first section, we created a slug for each of the products that we had in the store. And so **a slug is basically just a string that we can put in the URL, usually based on some string like the name.**  So in this case, we're gonna create **a slug based here on the tour name.** So remember how for that we used the slugify package. And so let's now go ahead and install that.

> models/tourModel.js

```js
// eslint-disable-next-line import/no-extraneous-dependencies
const slugify = require('slugify');

//.And remember this actually happened to us beforewhen we only had a couple of fields in the schema.And when we, then we'll define some fieldsthat were not in the schema, then they weresimply not persisted to the database.And the same thing is now happening here,so we defined the slug property, but it's not in our schema.
const tourSchema = new mongoose.Schema(
  {
   ...
    slug: String,
   ...
 
//DOCUMENT MIDDLEWARE : runs before .save() and .create() methods
tourSchema.pre('save', function (next) {
  // console.log(this);
  this.slug = slugify(this.name, { lower: true });
  next();
});

// eslint-disable-next-line prefer-arrow-callback
tourSchema.pre('save', function (next) {
  console.log('Will save document...');
  next();
});

// eslint-disable-next-line prefer-arrow-callback
tourSchema.post('save', function (doc, next) {
  console.log(doc);
  next();
});  
```

**multiplepre middlewares or also post middlewares for the same hook.And hook is what we call this save here. So this middleware here is basically what we calla pre save hook.**

**that this save middleware only runs for the saveand create Mongoose methods**.It's not gonna run, for example, for 	`insertmany` and also not for find one and `update or find by ID and update` which we already used before. So for example, we somewhere here we havewe have findByIdAndUpdate, but that is not gonna triggerthis save middleware.So that's very important to keep in mindbecause actually a bit later in this project,we will have to work around that limitation.All right, so this is document middlewareto manipulate documents that are currently being saved.Next up, we're gonna talk about query middleware.

[#Middleware mongoose ‼️](https://blog.csdn.net/caseywei/article/details/109524964)

## Query middleware(Mongoose)

let's now add a pre-find hook,so basically, a middleware that is gonna runbefore any find query is executed.So let's add another comment here.So this is query middleware.

And of course, it works in a very similar wayas before.So the tourSchema, and then pre,and in here we define the hook,which is gonna be "find."So, function, and again, we have accessto the next function.

```js
tourSchema.pre('find', function(next) {
```

let's suppose that we can havesecret tours in our database,like for tours that are only offered internally,or for a very small, like, VIP group of people,and that the public shouldn't know about.Now, since these tours are secret,we do not want the secret tours to ever appearin the result outputs.Right?

> models/tourModel.js

```js
const tourSchema = new mongoose.Schema(
  {
  ...
	secretTour: {
      type: Boolean,
      default: false
    }
  },
  ...
);
  
// QUERY MIDDLEWARE
// tourSchema.pre('find', function(next) {
tourSchema.pre(/^find/, function (next) {
  //find all document `secretTour` is not true
  this.find({ secretTour: { $ne: true } });

  this.start = Date.now();
  next();
});

tourSchema.post(/^find/, function (docs, next) {
  console.log(`Query took ${Date.now() - this.start} milliseconds!`);
  next();
});
```

/^find/ can run find, findOne, findOneandDelete.....

## Aggregation middleware

```js
// AGGREGATION MIDDLEWARE
tourSchema.pre('aggregate', function (next) {
  this.pipeline().unshift({ $match: { secretTour: { $ne: true } } });

  console.log(this.pipeline());
  next();
});
```

After sending this request at postman `127.0.0.1:3000/api/v1/tours/tour-stats`

![](https://res.cloudinary.com/dxmfrq4tk/image/upload/v1679366239/Screen_Shot_2023-03-20_at_9.36.21_PM_xzuwnn.png)

it matched the group with **controllers/tourController.js** routes 

```js
//Basically removing from the output all the documents that have secretTour set to true 
this.pipeline().unshift({ $match: { secretTour: { $ne: true } } });
```

## Data Validation: Built-In Validators

Well, validation is basically checking if the enteredvalues are in the right format for each fieldin our document schema, and also that valueshave actually been entered for all of the required fields.Now, on the other hand, we also have sanitization,which is to ensure that the inputted data isbasically clean, so that there is no malicious codebeing injected into our database,or into the application itself.So, in that step we remove unwanted characters,or even code, from the input data, all right?And this is actually a crucial step, like,a golden standard in back-end development.To never, ever accept input datacoming from a user as it is.So, we always need to sanitize that incoming data.But, anyway, I will leave data sanitizationfor the security section of the course,so that in this lecturewe can focus entirely on data validation.

And that, again, is because of the fat modeland thin controller philosophy,which makes the model the perfect placeto perform validation, right?And, in fact, Mongoose already comes with a bunchof validation tools out of the box.

```js

const tourSchema = new mongoose.Schema(
  {
    name: {
    ....
      maxlength: [40, 'A tour name must have less or equal then 40 characters'],
      minlength: [10, 'A tour name must have more or equal then 10 characters']
      // validate: [validator.isAlpha, 'Tour name must only contain characters']
    },
    ratingAverage: {
      .....
      min: [1, 'Rating must be above 1.0'],
      max: [5, 'Rating must be below 5.0'],
    },
```

```js
    difficulty: {
      type: String,
      required: [true, `A tour must have a difficulty`],
      enum: {
        values: ['easy', 'medium', 'difficult'],
        message: 'Difficulty is either: easy, medium, difficult',
      },
    },
```

## Data Validation: Custom Validators



which should return either true or false.And if it returns false, then it means there is an error.And on the other hand when we return true,then the validation is correctand the input can be accepted.Okay, so let's now build a simple custom validator here.

```js
 priceDiscount: {
      type: Number,
      validate: {
        validator: function(val) {
          // this only points to current doc on NEW document creation
          return val < this.price;
        },
        message: 'Discount price ({VALUE}) should be below regular price'
      }
    },
```

> `npm i validator` https://github.com/validatorjs/validator.js/

```js
const validator = require('validator');
...
//validate: [validator.isAlpha, 'Tour name must only contain characters']
```

# 🚧Error Handling with Express

## Debugging Node.js with ndb

### Installation

> package.json

```js
"debug": "ndb server.js"
```

```js
sudo npm i ndb
sudo npm run debug
```

## Handling Unhandled route

So for doing that remember that all these middleware functions are executed in the order they are in the code. so this should basically be the last part after all our other routes, 

```js
// implement route handler, handler all routes, http methods
app.all('*', (req, res, next) => {
  res.status(404).json({
    status: 'failed',
    message: `Can't find ${req.originalUrl} on this server`,
  });
});
```

##  An Overview of Error Handling

![](https://res.cloudinary.com/dxmfrq4tk/image/upload/v1679465938/Screen_Shot_2023-03-22_at_1.18.50_AM_nmzdun.png)

## Implementing a Global Error Handling Middleware

we have request, response, and next.So by specifying four parameters, Express automatically knows that this entire function here is an error handling middleware.

```js
app.use((err, req, res, next) => {});
```

So we say that the error.statuscode is equalto err.statuscode basically if it is defined or the code 500, which means again,internal server error, and so that's usually the standard that we use.And in the same way, we also define the status,so let's say **error.status** is equal to error.status if it is defined, and if not,it's error So error, remember, is when we have a 500 status codeand if it's a 400 status code, then it's a fail.So for example here in this 404 , status is fail.And so now here we can then use that,so error.statuscode, and then send some json,so very similar to what we did before.

```js
 err.statusCode = err.statusCode || 500;
 err.status = err.status || 'error';
```

```js
//error middleware
app.use((err, req, res, next) => {
  //internal server error 500
  err.statusCode = err.statusCode || 500;
  err.status = err.status || 'error';

  res.status(err.statusCode).json({
    status: err.status,
    message: err.message,
  });
});
```

```js
app.all('*', (req, res, next) => {
 
  //creating a new Error object
  const err = new Error(`Can't find ${req.originalUrl} on this server`);
  err.status = 'failed';
  err.statusCode = 404;
  
  
  //we need to pass the err into next()
  next(err);
  //this will trigger error middleware
});
```

## Create own Errors Class and Refactoring

In this video, let's now createa better and more useful error class,and also do some refactoring.And starting with that error class,let's create a new file in our Utilities folder. So, new file, and I'm going to call it AppError because that's going to be the name of the class, all right? So, class AppError, and we actually want all of our AppError objects to then inherit from the built-in error,and so let's actually extend the built-in error class. 

```js
class AppError extends Error {
  // so, remember the constructor method is calledeach time that we create a new object out of this class.
  constructor(message, statusCode) {
    super(message);

    this.statusCode = statusCode;
    //the statusCode as a stringstarts with a four, well, then we have a fail.And so here, let's use the ternary, okay,and so it's fail when it starts with a four,and otherwise it's an error,
    this.status = `${statusCode}`.startsWith('4') ? 'fail' : 'error';
    //So all of our errors willget this property set to true,and I'm doing that so that laterwe can then test for this propertyand only send error messages backto the client for these operational errorsthat we created using this class.And this is useful because some other crazy unexpected errors that might happenin our application, for example a programming error,or some bug in one of the packagesthat we require into our app,and these errors will then of coursenot have this .is operational property on them.
    this.isOperational = true;
  }
}
```

> utils/appError.js

Stacktrace show where the error happened, also will show the all call stacks

```js
console.log(err.stack);
```







## Catching Errors in Async Functions

## Adding 404 Not Found Errors

## Errors During Development vs Production

## Handling Invalid Database IDs

## Handling Duplicate Database Fields

## Handling Mongoose Validation Errors

- 

  ##  Errors Outside Express: Unhandled Rejections

  
