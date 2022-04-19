# redis-express-
basices comands+setUp +connction+api 


### COMMAND LINE ACTIONS AFTER CONNECTION:</br>
#### redis general commands: 
<pre>
redis 127.0.0.1:6379> PING
PONG
redis 127.0.0.1:6379> SET name elad
OK
redis 127.0.0.1:6379> GET name
"elad"
redis 127.0.0.1:6379> SET age 26
OK
redis 127.0.0.1:6379> Get age
"26"
redis 127.0.0.1:6379> DEL age
(integer) 1
redis 127.0.0.1:6379> Get age
(nil)
redis 127.0.0.1:6379> EXISTS bru
(integer) 0
redis 127.0.0.1:6379> EXISTS name
(integer) 1
redis 127.0.0.1:6379> KEYS *
1) "name"
2) "post"
redis 127.0.0.1:6379> FLUSHALL
OK
redis 127.0.0.1:6379> KEYS *
(empty list or set)
redis 127.0.0.1:6379> SET name elad
OK
redis 127.0.0.1:6379> GET name
"elad"
</pre>

#### Handling Expirations

<pre>
redis 127.0.0.1:6379> ttl name 
/*ttl-timeToLive*/
(integer) -1//not have ttl 
redis 127.0.0.1:6379> expire name 10 //set 10 secto expired
(integer) 1
redis 127.0.0.1:6379> ttl name
(integer) 6
redis 127.0.0.1:6379> ttl name
(integer) 4
redis 127.0.0.1:6379> ttl name
(integer) 3
redis 127.0.0.1:6379> ttl name
(integer) 1
redis 127.0.0.1:6379> ttl name
(integer) -1//it means the key expired
redis 127.0.0.1:6379> get name
(nil)//now its not exicts

/*second way to execute exparation when creating  spesifice  key*/
redis 127.0.0.1:6379> setex name 10 elad
OK
redis 127.0.0.1:6379> ttl name
(integer) 5
redis 127.0.0.1:6379> ttl name
(integer) 3
redis 127.0.0.1:6379> ttl name
(integer) 0
redis 127.0.0.1:6379> ttl name
(integer) -1
redis 127.0.0.1:6379> ttl name
(integer) -1
redis 127.0.0.1:6379> get name
(nil)

</pre>
#### Lists: 
<pre>
redis 127.0.0.1:6379> lpush friends ross //create new list name friends and add ross in it 
(integer) 1
redis 127.0.0.1:6379> lpush friends monika //add monika to thefirst cellof the list 
(integer) 2
redis 127.0.0.1:6379> lrange friends 0 -1 //show all list rang from 0 to the end of it
1) "monika"
2) "ross"
redis 127.0.0.1:6379> rpush friends chendler //add chendler to the last cell of the list
(integer) 3
redis 127.0.0.1:6379> lrange friends 0 -1 //show the new list 
1) "monika"
2) "ross"
3) "chendler"
redis 127.0.0.1:6379> LPOP friends //remove the obj of the first cell that is from the left 
"monika"
redis 127.0.0.1:6379> lrange friends 0 -1
1) "ross"
2) "chendler"
redis 127.0.0.1:6379> RPOP friends //remove the obj of the last cell that is from the right 
"chendler"
redis 127.0.0.1:6379> lrange friends 0 -1
1) "ross"



</pre>


#### Sets: 
<pre>



</pre>
<!-- https://www.youtube.com/watch?v=jgpVdJB2sKQ -->
<!-- https://www.youtube.com/watch?v=KUfufrwpBkM -->
<!--  -->


