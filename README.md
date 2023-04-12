
## Ali_Khatami_Solidity5
### Contract deploying Contract
The ability of contracts interacting with each other is known as composability. Smart Contracts are composeble because they can easily interact with each other.<br>
In case of DeFi, complex financial products interact with each other easily as all their code are available on chain.<br>

![w9](https://user-images.githubusercontent.com/89090776/230866485-06fb95f3-7fb7-4802-9de1-aa664aed3baf.jpg)
Figure1: here we will create a new solidity file which is ```akrkStorageFactory.sol```

![w10](https://user-images.githubusercontent.com/89090776/230868690-f1122ce1-24e4-4b04-93e0-266985b3fbd3.jpg)
Figure2: we will copy everything below ```pragma solidity ^0.8.7``` of ```akrkSimplkestorage.sol``` file  and paste it to ```akrkStoragefactory.sol``` file before<br>
```contract akrkStoragefactory``` line and now the ```akrkStoragefactory.sol``` will have two contracts.<br>

Single solidity file can hold multiple different contracts. here is the code below:

```
//SPDX-License-Identifier:MIT
pragma solidity ^0.8.0;

contract akrkSimplestorage {
  // basic data types: boolean, uint,int,address,bytes
  //uint(unsigned integer only positive),int both pos and neg
  //address is the address of the account
  //string are secretly byte objects only for text
  //bytes32 is a bytes objects whre 32 represent how many bytes we want them to be,max size is 32
  //byte objects look like 0xsdsysydggt
  //unint256 here 256 is bit (8,16,32 upto 256 we can use)
  uint256 preferredNumber;//remove public so that we don't get duplicate functions at the moment we will just get the retrieve function
   //if we have whole bunch of variables inside the contract akrkSimplestorage those actually get indexed
   //here 'uint256 preferredNumber' will get index to 'zero'
//below is type mapping of 'string' to 'uint256', visibility is 'public'
   mapping(string => uint256) public nameToPreferredNumber;//Here we have kind of dictionary where we gonna map a specific name to a specific number
  //When we create mapping we initiaize everything to it's null value so every possible string is initialize to having preferredNumber to zero, for that we have to manually change it.
//Below we have created a new type in our solidity that holds both preferred number and name 
  struct Individuals{
    uint256 preferredNumber;//this get indexed to 0
    string name;// this get indexexd to 1
//whenever we have list of variables inside an object they automatically get indexed

}

  // here below we create an array of uint256 and now preferred number can be a list or array
  //uint256[] public preferredNumbersList;

//here below we want an array of Individuals we give it visibility of public and variable name individuals
  Individuals[] public individuals; //it is automatically giving a view function and it will give nothing if it is empty and in it's button the value that it wants is gonna be the index of the object
// array above is a dynamic array because size of the array is not given at it's initialization if we don't add any size it can be any size and here we gonna work with dynamic array

//pasing parameter of type uint256 and made the function public
  function store(uint256 _preferredNumber) public{
      preferredNumber = _preferredNumber;
    
  }

  function retrieve() public view returns(uint256){
    return preferredNumber;
  }

  //now we will create a function that is going to add individuals to Individual array
  function addCitizen(string memory _name, uint256 _preferredNumber) public {
      
      individuals.push(Individuals(_preferredNumber,_name));//Here we have created a push function which is available in our Individuals object
      nameToPreferredNumber[_name]=_preferredNumber ;//here we are going to add Individual to the individuals array where we add key name equal to preferred number
      // I have remove the semicolon from the above code 
      //Here in the above inside the push function we have created a new Individual object that will take preferred number and name. 
  //push Individual in our individual array ,we can also do in this way
  }
  //We don't need the 'name' variable after the above function runs we can keep it as memory
  //We can also keep the name as 'calldata' in that case we can't modify it 


  
}


contract akrkStoragefactory {

    akrkSimplestorage public akrksimplestorage; //here we have created a global variable 

    function createakrkSimplestorage() public{
        akrksimplestorage = new akrkSimplestorage();
       //here `new` keyword is used to let the solidiy know that we are going to deploy new akrkSimplestorage()

    } // This is a function to deploy our akrkSimplestorage contract

}

```

In the bove code whenever we will call ```function createakrkSimplestorage() public``` it will replace whatever currently is in ```akrkSimplestorage public akrksimplestorage;``` variable

![w11](https://user-images.githubusercontent.com/89090776/230873674-91a0d63c-cfda-4600-b3f7-cbf17ed6a004.jpg)
Figure3: here we will see that two buttons have been created one is due to ```createakrkSimplestorage()``` which is ```createakrkSimplestorage``` buuton<br>
it is a gas calling function and another one is ```akrkSimplestorage``` button which is due to our global variable ```akrkSimplestorage public akrksimplestorage```  and it is a non gas calling function.

![w12](https://user-images.githubusercontent.com/89090776/231064667-6b01615f-9fec-4351-b8ce-33e53cdcb77c.jpg)
Figure4: Here when we hit the ```akrkSimplestorage``` button it shows the address blank because ```akrkSimplestorage public akrksimplestorage``` is initialized to blank.

![w13](https://user-images.githubusercontent.com/89090776/231066290-f4ad2505-837b-41b7-9a8d-ff59fd813981.jpg)
Figure5: here we see we after clicking ```createakrkSimplestorage``` button we have created a new function at our console which is<br> ```akrkStoragefactory.createakrkSimplestorage()``` and by doing so we have created a new ```akrkSimplestorage``` contract.

![w15](https://user-images.githubusercontent.com/89090776/231067145-e3a1b2d9-9608-455b-9241-d0531e595862.jpg)
Figure6: Now if we click the ```akrkSimplestorage``` button we can see the address associted with it.And here we understood how a contract can deploy another contract.


another aternative is given below:

```
//SPDX-License-Identifier:MIT
pragma solidity ^0.8.0;

import "./akrkSimplestorage.sol";

contract akrkStoragefactory {

    akrkSimplestorage public akrksimplestorage; //here we have created a global variable 

    function createakrkSimplestorage() public{
        akrksimplestorage = new akrkSimplestorage();
       //here `new` keyword is used to let the solidiy know that we are going to deploy new akrkSimplestorage()

    } // This is a function to deploy our akrkSimplestorage contract

}
```

here in the above code of ```akrkStorageFactory.sol```the line  ```import ./akrkSimplestorage.sol``` is exact the same as the copy paste of ```akrkSimplestorage.sol``` file<br>
i.e it takes the path of ```akrkSimplestorage.sol``` and paste all code of this file at ```akrkStorageFactory.sol```


![w16](https://user-images.githubusercontent.com/89090776/231071349-512178a6-c6e7-4fa0-af93-8e68b5f32cc5.jpg)
Figure7: here we see that that the contract ```contract akrkStoragefactory``` have deployed contract ```contract akrkSimplestorage``` as before. 

When a contract of a file deploy contract of another file the solidity version of both file must be compatible i.e if version of of a file is ^0.8.0 other <br>
must not be ^0.7.0<br>


Here we have worked with array

```
//SPDX-License-Identifier:MIT
pragma solidity ^0.8.0;

import "./akrkSimplestorage.sol";

contract akrkStoragefactory {

    akrkSimplestorage[] public akrksimplestorageArray; ////here we have created an array to store addresses of the contracts


    function createakrkSimplestorage() public{
        akrkSimplestorage akrksimplestorage = new akrkSimplestorage();// here we save it as a memory variable
       //here `new` keyword is used to let the solidiy know that we are going to deploy new akrkSimplestorage()


       akrksimplestorageArray.push(akrksimplestorage);// adding akrksimplestorage variable to akrksimplestorageArray



    } // This is a function to deploy our akrkSimplestorage contract

}

```

![w17](https://user-images.githubusercontent.com/89090776/231091399-87e97613-e150-4939-a324-9e6bd8a97986.jpg)
Figure8: after deploying the contract we see that we have ```akrksimplestorageArray``` view button
![w18](https://user-images.githubusercontent.com/89090776/231092783-1a901de0-65bd-481e-9347-2e89f14ac274.jpg)
Figure9: here when we click ```createakrkSimplestorage``` button transaction occured that means contract was deployed and then we typed '0' at the field of <br> ```akrksimplestorageArray``` button and see that at '0' inex an address of the contract have been assinged.

![w19](https://user-images.githubusercontent.com/89090776/231094487-f568517d-ca21-4724-91ec-20c90317206f.jpg)
Figure10: Again we click on ```createakrkSimplestorage``` button transaction will occur that means contract will be deployed and if we typed '1' at the field of <br> ```akrksimplestorageArray``` button and see that at '1' inex a new address of the contract have been assinged.



<br><br>



### Interacting with other contracts

Here we will show that how to call a function of ```akrkSimplestorage.sol``` file  from ```akrkStorageFactory.sol``` file. In order to interact with any contract <br>
we always gonna need two things the address of the contract and ABI of the contract <br>
ABI stands for Application Binary Interface and it will tell our code of how exactly it will interact with the contract and it will tell all inputs and <br>
outputs and all we can do with this contract

![w20](https://user-images.githubusercontent.com/89090776/231108130-f8d3cdc4-e1db-4286-996a-c375de287c36.jpg)
Figure11: now we will go to ```Solidity Compiler``` tab and switch to contract ```akrkSimplestorage``` we will see below ```Compilation Details``` <br>

![w21](https://user-images.githubusercontent.com/89090776/231110285-bc465008-acda-4124-934f-470e6098f4fc.jpg)
Figure12: here we will click ```ABI``` and here we will get four different ways to inteerct with the contract and details of each ways.<br>

We will get addresses at ```akrkSimplestorage[] public akrksimplestorageArray;``` and ABI because of this line ```import "./akrkSimplestorage.sol";```


```
//SPDX-License-Identifier:MIT
pragma solidity ^0.8.0;

import "./akrkSimplestorage.sol";

contract akrkStoragefactory {

    akrkSimplestorage[] public akrksimplestorageArray; //here we have created an array to store addresses of the contracts

    function createakrkSimplestorage() public{
        akrkSimplestorage akrksimplestorage = new akrkSimplestorage();// here we save it as a memory variable
       //here `new` keyword is used to let the solidiy know that we are going to deploy new akrkSimplestorage()


       akrksimplestorageArray.push(akrksimplestorage);// adding akrksimplestorage variable to akrksimplestorageArray



    } // This is a function to deploy our akrkSimplestorage contract


    function asfStore(uint256 _akrksimplestorageIndex,uint256 _akrksimplestorageNumber) public {

        akrkSimplestorage akrksimplestorage =  akrksimplestorageArray[_akrksimplestorageIndex];  //We are saving akrkSimplestorage contract object at 'akrksimplestorageIndex' to our 'akrksimplestorage' variable

        akrksimplestorage.store(_akrksimplestorageNumber);// Calling the store function of 'akrkSimplestorage' contract

    }

    //Below we will create a function that gonna read from 'akrksimplestorage' contract 

    function sfGet(uint256 _akrksimplestorageIndex) public view returns(uint256) {
        akrkSimplestorage akrksimplestorage =  akrksimplestorageArray[_akrksimplestorageIndex];

        return akrksimplestorage.retrieve();// to get the number we stored at the previous function

    }

}

```

After deploying the above code we will get the following output

![w22](https://user-images.githubusercontent.com/89090776/231353554-25204606-2551-45ca-897a-b90584d64d2a.jpg)















