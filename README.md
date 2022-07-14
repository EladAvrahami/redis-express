# redis-express
Basices comands+setUp+connction+api 


### COMMAND LINE ACTIONS AFTER CONNECTION:</br>
#### redis general cli commands (after starting redis server) : 
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
#### Sets (unique values) : 
<pre>
redis 127.0.0.1:6379> SADD hobbies "weight lifting" //add new set called hobbies
(integer) 1
redis 127.0.0.1:6379> SMEMBERS hobbies //see all hobbies
1) "weight lifting"
redis 127.0.0.1:6379> SREM hobbies "weight lifting" //remove spesific
(integer) 1
redis 127.0.0.1:6379> SMEMBERS hobbies
(empty list or set)
</pre>


### code ex1:
<pre>
// code example from: https://www.youtube.com/watch?v=AzQ6_DTcG6c
/*const express =require("express");
const redis = require("redis")
const util=require("util")
const axios = require ("axios");

const redisUrl="redis://127.0.0.1:6379"
const client= redis.createClient(redisUrl)

client.set=util.promisify(client.set)//inorder to be able using pormis with redis 
client.get=util.promisify(client.get)

const app =express();
app.use(express.json());

app.post("/",async(req,res)=>{
    const{key,value}=req.body;
    const response= await client.set(key,value)
    res.json(response)
})

app.get("/", async (req,res)=>{
    const{key} =req.body;
    const value= await client.get(key);
    res.json(value)
})

//example 1 : 
app.get("/posts/:id",async(req,res)=>{
    const {id}=req.params;
    //make uniqe key
    const cachedPost=await client.get(`post-${id}`)
    //check if it alredy found the key name before
    if(cachedPost){
        return res.json(JSON.parse(cachedPost))
    } 
    //if unique key not found get response from api and set new uniqe key
    const response=await axios.get(`https://jsonplaceholder.typicode.com/posts/${id}`)//frome: https://jsonplaceholder.typicode.com/
    client.set(`post-${id}`,JSON.stringify(response.data))
    return res.json(response)
})


//example 2 : 
//update cache using expired- in order to get new data to cach after changes in the api/DB 
app.get("/posts/:id",async(req,res)=>{
    const {id}=req.params;
    const cachedPost=await client.get(`posts-${id}`)
    if(cachedPost){
        return res.json(JSON.parse(cachedPost))
    } 
    const response=await axios.get(`https://jsonplaceholder.typicode.com/posts/${id}`)
    // SET a key that its exparation date will be in 10 sec
    client.set(`post-${id}`,JSON.stringify(response.data),"EX",10)
    return res.json(response)
})

//setting port 
var PORT=8080;
app.listen(PORT,()=>{
    console.log(`listen to port ${PORT} !`)
});*/


</pre>


### code ex2:
<pre>


/******************************** NEW CODE 2 ***************************************************/
//code +readme file basice coomands on redis from : https://www.youtube.com/watch?v=jgpVdJB2sKQ
const express =require("express");
const axios = require ("axios");
const cors =require("cors");
const Redis = require("redis")

//help me get the option to use that var with redis commands
const redisClient=Redis.createClient()
//one Hour
const DEFUALT_EXPERATION=3600

 

const app =express();
app.use(cors())

// #1 GET req => http://localhost:3000/photos
app.get("/photos",async(req,res)=>{
    const albumId=req.query.albumId;
    //change key to spesific alboum id👇 while doing param query
    RedisClient.get(`photos?albumId=${albumId}`,async(error,photos)=>{//req redis photos collection //19:00
        if (error) console.error(error)  
        if(photos!=null) {//it means that redis still hold the last data it got 
            console.log('cache hit') 
            return res.json(JSON.parse(photos))//so return this data 
        }else{
            console.log('cache Miss') 
            // execute this code below if photos obj expired/not init at first call yet...
            const { data } = await axios.get("https://jsonplaceholder.typicode.com/photos", 
            {params:{albumId}}
            )
            //use redisClient that i set above and add expration to 
            //data is coming from axios as an array so i need to make it to string(stringify) in order to sent it to redis  
            redisClient.setex(`photos?albumId=${albumId}`,DEFUALT_EXPERATION,JSON.stringify(data))
            res.json(data)
        }
       
     })   
})



//#2
app.get('/photos/:id', async(req,res)=>{
    const {data } = await axios.get(
        `https://jsonplaceholder.typicode.com/photos/${req.params.id}`
    )
    res.json(data);
});



//#4 write  stage #1 in shorter way using function

app.get("/photos",async(req,res)=>{
    const photos=await getOrSetCache(`photos?albumId=${albumId}`,async()=>{
        const { data } = await axios.get("https://jsonplaceholder.typicode.com/photos", 
        {params:{albumId}}
        )  
        return data
    })
   res.json(photos)
})



//#3
//EXTERNAL FUNCTION -do the process of checking key validation and define where to go redis or api  
function getOrSetCache(key,cb){
    return new Promise((resolve,reject)=>{
        //try get key if not return eror else return key data
        redisClient.get(key, async(error,data)=>{
            if (error) return reject(error);
            if (data!=null) return resolve(JSON.parse(data))
            const freshData=cb()//the new data in case it is first try getting to DB or last call expired 
            redisClient,setex(key,DEFUALT_EXPERATION,JSON.stringify(freshData))
            resolve(freshData)
        })

    })
}




console.log("work ")
app.listen(3000);

</pre>




<!-- helpful linkes -->
<!-- https://www.youtube.com/watch?v=jgpVdJB2sKQ -->
<!-- https://www.youtube.com/watch?v=KUfufrwpBkM -->
<!-- https://www.youtube.com/watch?v=AzQ6_DTcG6c -->
<!-- https://www.youtube.com/watch?v=OCOWjTPu9DI&list=PLS1QulWo1RIYEMepIBinxplXsx9v0zCSf&index=18 -->

<!--  -->


