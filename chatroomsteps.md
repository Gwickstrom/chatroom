1. Starting with only the index.ejs file, you're going to add jquery and socket.io to script tags.
    - $(document).ready(function(){
        //connect to socket.
        //invoke current_user

2. //Declare a function (new_user) that, when called, prompts the user to input a name, and emit an event with the name of the event, "page_load" in this case, along with the key:value pair {user: name}.
        }).

3. Call new_user();

4. Have the client listen for events from the server, such as: socket.on("existing_user", function(data){
    $('error').html(data.error)
    //Listens for event called existing_user, in the case of an error, responds with a jQuery created event $('error').html(data.error)
    //Then call new_user function again to make sure the user can login again if they want to.
    new_user();
    })


//NOTE TO SELF, figure out what exactly gets passed in the data object.

5. Have the client listen for event from the server,
    socket.on('load_messages', function(data){
        $('error').html(); //RESETTING THE ERROR MESSAGE
        current_user = data.current_user;
        var messages_thread = "";

        for(var i=0; i < messages.length; i++){
            messages_thread += "<p>" + messages[i].name + ": " + messages[i].message + "</p>";
        }

        $('message_board'.append(messages_thread);
    })

6. jQuery submit functions that serve to submit a new_message. The information regarding the new_mesage can be found in the emit itself.
    Ex:

    $('#new_message').submit(function(){
        socket.emit('new_message', {message: $('message').val(), user: current_user })
        return false;
    })

7. Client listens for event called "post_new_message" that contains the
function(data){
    $("#message_board").append("<p>" + data.user + ": " + data.new_message + "</p>");
}

8. In the body of the ejs file, must make divs with ids of container, error, messsage_board. Along with a form tag with the id new_message.
    The input type of the form has an id of "message".
    The input type of submit has a value of "send".
