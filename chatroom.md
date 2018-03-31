0. ALWAYS DO HTML CSS jQuery first.

1. Have the NodeJS server render views/index.ejs that has the html content for the client whenever the client requests "/".
    routes/index.js

    // This goes in the routes/index.js file
    app.get('/', function(req, res){
        res.render('index', {});
    });

2. In the client code, have a javascript code that asks the user for their name and store this user input in a variable called name.

    //index.ejs
    <script>
        var name = prompt("what is your name?");
    </script>

3. Have the client EMIT "got_a_new_user" and pass "name" to the server.

    //index.ejs
    <script>
        var name = prompt("What is your name?");
        io = io.connect();
        io.emit('got_a_new_user', {name: name});
    </script>


4. Have the server listen for an event called "got_a_new_user" and when that event gets triggered what should we have it do?
    io.on('got_a_new_user', function(name){
        
        })
    1. Have the server broadcast an event called "show_the_new_user" to the client and pass the data, naame of the new user attached to this event.
        app.io.route('got_a_new_user', function(req){
            req.data.name
            app.io.broadcast('show_the_new_user', {new_user_name: req.data.name});
            });
    2. We store the name/session_id of the new user in a variable/array/hash called users.

        var users = {};


    3. To the person who sent this request, we EMIT an event called 'existing_users' with all the users data.

        app.io.route('got_a_new_user', function(req){
            req.data.name
            app.io.broadcast('show_the_new_user', {new_user_name: req.data.name});
            req.io.emit('existing_users');
            });


5. Have the client listen for an event called "show_the_new_user" and when this event gets triggered, have the client render this information in jQuery somewhere in the HTML.

    <script>
        var name = prompt("What is your name?");
        io = io.connect();
        io.emit('got_a_new_user', {name: name});

        io.on('got_a_new_user', function(data){
            //render this info to HTML
        })
    </script>




6. Have the server listen for an event called  "disconnect", and when this occurs, BROADCAST to all clients an event 'disconnect_user' with some data (name of the disconnected user). (NOTE: we'll probably need to pass session id or something else to identify a user)

    app.io.route('disconnect', function(req){
        req.data.name
        app.io.emit('disconnect_user', {new_user_name: req.data.name});
    });


7. Have the client listen for an event 'disconnect_user' and when this gets triggered, have the client remove the proper jQuery box for that user.

    <script>
        var name = prompt("What is your name?");
        io = io.connect();
        io.emit('got_a_new_user', {name: name});

        io.on('got_a_new_user', function(data){
            //render this info to HTML
        })
        io.on('disconnect_user', function(data){
            //remove jQuery box for that user that disconnected
        })
    </script>
