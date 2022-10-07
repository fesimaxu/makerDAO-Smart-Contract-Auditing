## MakerDAO DssVest.sol Repo Review





# Pragma solidity 0.6.12 : The compiler version used to compile # # the contract by the EVM compiler.

[2022-10-07 03-48-08.png]
# Interface : are contracts that contains function signatures without the function definition implementation and are identified using the “interface” keyword.
# External : It is a function visibility that the function can only be access from outside its existing contract.
# View : It is a function state mutability that promise not to modify the state but can only read the state.
# Interface MintLike : Provides a minting function interface to mint token.
# mint: function mint is a external function signature of MintLike for minting token with address(user address) and uint256(amount). 
# Interface ChainlogLike : Provides a function signature to read a bytes32 state variable and returns address as the output.
# getAddress : function getAddress is an external view function signature of ChainlogLike that accepts a bytes32 parameter and returns address as output. 
Interface DaiJoinLike : Provides a function signature to exit contracts.
exit : function exit is an external function signature of DaiJoinLike that accepts address and uint256 as parameter to trigger exit operation in a contract.
Interface VatLike : Provides three(3) function signatures of hope,suck and live to perform operation on address, uint256 and read a state variable.
hope : function hope is an external function signature that accepts an address as a parameter.
suck : function suck is an external function signature that accepts address from, address to and uint256 amount.
live : function live is an external view function signature that returns uint256(amount) as ouput value.
Interface TokenLike : Provides a function signature of transferFrom.
transferFrom : function transferFrom is an external function signature thats accepts parameters of address from, address to and unit256 amount which allows transfer of amount between token accounts within a contract and returns true on success and false on failure.
CONTRACT

Contract DssVest : Is an abstract contract in which at least one of its function interface is not implemented and it is marked with the keyword “abstract”.

Mapping : Is a reference type variable.
Public : It is a state visibility that a function or state variable can be access within and outside its existing contract.
wards : Is a mapping with a visibility of public, key type of address and a value type of uint256,located at position 0 and occupies a 32bytes storage slot.

awards : Is a mapping with a visibility of public , key type of uint256 and value type of struct (Award), located at postion 2 and occupies a 32bytes storage slot.
Struct

Award : Is a struct located at position 1, occupies 3 storage slot and contains the following state variables
address usr : as vesting recipient and on the first storage slot.
uint48 bgn : as the start vesting period and on the first storage slot.
uint48 clf : as the cliff date and on the first storage slot.
uint48 fin : as the end of vesting period and on the 2nd storage slot.
address mgr : as the manager address that can control the contract and on the 2nd storage slot.
uint8 res : as the restriction variable and on the 2nd storage slot.
uint128 tot : as the total reward amount from the vest and on the 3rd storage slot.
uint128 rxd : as the amount of vest claimed and on the 3rd storage slot.

State Variables : are value type data types declared as global variables with in a contract which can have state visibility of either public, internal or private.
Internal : It is a state visibility that a function or state variable can be access only within its existing contract.
Constant : are state variables initialized at compliation time and copied to every location of the state variable. It is used to save gas.
Cap : a value type uint256 state variable, located at postion 3 and occupies 1 storage slot of 32 bytes. It has state visibility of public and It is the maximum per-second issuance token rate of the contract.
ids : a value type uint256 state variable, located at position 4 and occupies 1 storage slot of 32 bytes. It has state visibility of public and It is the total vestings of the contract.
locked :  a value type uint256 state variable, located at position 5 and occupies 1 storage slot of 32 bytes. It has state visibility of internal.
TWENTY_YEARS :  a constant value type uint256 state variable, located at position 6 and occupies 1 storage slot of 32 bytes. It has state visibility of public and it is the time period in which the vesting will last.
Events : It is used to allow logging to the ethereum blockchain.

Event Rely : To emit/log addresses.
Event Deny : To emit addresses.
Event File : To emit byte32 and uint256 values.
Event Init : To emit uint256 and address values.
Event Vest : To emit uint256 values.
Event Restrict : To log uint256 values.
Event Unrestrict : To log uint256 values.
Event Yank : To log uint256 values.
Event Move : To log uint256 values.

Constructor : Initialized the msg.sender as the owner at construction time and emit the address of the msg.sender using the “Event Rely”.
MODIFIER

lock : A modifier to lock the vest.
    • It checks that  “locked” is false with “require” keyword. 
    • sets “locked” to true in order to lock the function.
    • “_;” allows for the execution of other codes of the function and then set “locked” to false.
auth : A modifier to check that msg.sender is owner of the contract.
    • It checks that  “msg.sender” is owner with  “require” keyword. 
    • “_;” allows for the execution of other codes of the function.

FUNCTIONS

usr : Is an external view function that accepts uint256 value as a parameter and returns vesting recipient address as output, using the mapping “awards” to access the struct “Award” value with its “id”.
bgn : Is an external view function that accepts uint256 value as a parameter and returns a uint256 start of vesting period as output, using the mapping “awards” to access the struct “Award” value with its “id”.
clf : Is an external view function that accepts uint256 value as a parameter and returns a uint256 cliff date as output, using the mapping “awards” to access the struct “Award” value with its “id”.
fin : Is an external view function that accepts uint256 value as a parameter and returns a uint256 end of vesting period as output, using the mapping “awards” to access the struct “Award” value with its “id”.
mgr : Is an external view function that accepts uint256 value as a parameter and returns an address that controls the vesting as output, using the mapping “awards” to access the struct “Award” value with its “id”.
res : Is an external view function that accepts uint256 value as a parameter and returns a uint256 value of the restriction placed on the vesting as output, using the mapping “awards” to access the struct “Award” value with its “id”.
tot : Is an external view function that accepts uint256 value as a parameter and returns uint256 total amount as output, using the mapping “awards” to access the struct “Award” value with its “id”.
rxd : Is a view function that accepts uint256 value as a parameter and returns a unit256 of amount of vest claimed, using the mapping “awards” to access the struct “Award” value with its “id”.

create : It is a function used to add the details of a new user in the contract with the following checks on the parameters.
    • It checks that user address is not address zero.
    • It checks that “_tot” is greater than zero.
    • It checks that “bgn” is greater than the addition of “block.timestamp” and the statevariable  “TWENTY_YEARS”.
    • It checks that “bgn” is greater than the subtraction of “block.timestamp” and the statevariable  “TWENTY_YEARS”.
    • It checks that “_tau” is greater than zero.
    • It checks that the division of “_tot” and “_tau” is less or equal to the cap (maximum per-second issuance token rate).
    • It checks that “_eta” is less or equal to the “_tau”.
    • It checkks that “ids” is less than the maximum of uint256.
    • It pre increase the id count.
    • It creates a new user details with the struct “Award” values by accessing it with mapping “award”.
    • It emits the “Init” event.


rely : It is an external function.
    • It authentic the user’s address as a registered address on the contract
    • It can only be trigger by contract owner.
    • It emits user’s address
deny : It is an external function.
    • It blacklist user’s address.
    • It can only be trigger by contract owner.
    • It emits user’s address.

file : It is an external function that sets the per second issuance rate of the vest contract.
    • It is accepts two parameters of bytes32 and uint256.
    • It emits File event.





Pure Function : It is a function that neither modifies or read storage state. 
min : It is an internal pure function that returns uint256.
    • It accepts two uint256 parameters as input.
    • It returns the value of the second parameter if it is less than the first parameter else it returns the first parameter.
add : It is an internal pure funtion that returns uint256 value.
    • It accepts two uint256 parameters as input.
    • It adds the two parameters and compare if it is greater or equal to the first parameter.
sub: It is an internal pure funtion that returns uint256 value.
    • It accepts two uint256 parameters as input.
    • It subtracts the two parameters and compare if it is less or equal to the first parameter.
mul : It is an internal pure funtion that returns uint256 value.
    • It accepts two uint256 parameters as input.
    • It  returns the product of the parameter.
touint48 : It is an internal pure funtion that returns uint256 value.
    • It accepts two uint256 parameters as input.
    • It type cast the uint256 parameter to uint48.
touint128 : It is an internal pure funtion that returns uint256 value.
    • It accepts two uint256 parameters as input.
    • It type cast the uint256 parameter to uint128.



vest : an external function for claiming rewards with user’s id.
vest : an external function overload for claming rewards with user’s id and maximum amount.
_vest : An internal function using lock modifier to lock the function, 
    • It access the struct “Award” details with mapping “awards” and its input argument “id” .
    • It checks that user’s address is not address zero.
    • It checks that msg.sender is the resgistered address on the contract.
    • It calls “unpaid function”  to get the unpaid tokens of the address(msg.sender).
    • It calls “min function” which returns maxamt if amt is less than maxamount.
    • It checks that the unpaid token is not greater than balance of the address token.
    • It trigger the “pay function” to excute withdraw of unpaid tokens to the user’s address.
