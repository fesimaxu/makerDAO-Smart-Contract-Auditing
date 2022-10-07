#                            MakerDAO DssVest.sol Repo Review

## Pragma solidity 0.6.12 
```solidity
pragma solidity 0.6.12;
```
#### The compiler version used to compile the contract by the EVM compiler.

## Interface 
#### are contracts that contains function signatures without the function definition implementation and are identified using the “interface” keyword.
```solidity
interface MintLike {
    function mint(address, uint256) external;
}

interface ChainlogLike {
    function getAddress(bytes32) external view returns (address);
}

interface DaiJoinLike {
    function exit(address, uint256) external;
}

interface VatLike {
    function hope(address) external;
    function suck(address, address, uint256) external;
    function live() external view returns (uint256);
}

interface TokenLike {
    function transferFrom(address, address, uint256) external returns (bool);
}
```
## External Function 
#### It is a function visibility that the function can only be access from outside its existing contract.

## View Function 
#### It is a function state mutability that promise not to modify the state but can only read the state.

#### Interface MintLike 
#### Provides a minting function interface to mint token.

#### _mint: function mint is a external function signature of MintLike for minting token with address(user address) and uint256(amount)._

## Interface ChainlogLike 
#### Provides a function signature to read a bytes32 state variable and returns address as the output.

#### _getAddress : function getAddress is an external view function signature of ChainlogLike that accepts a bytes32 parameter and returns address as output._ 

#### Interface DaiJoinLike 
### Provides a function signature to exit contracts.
#### _exit : function exit is an external function signature of DaiJoinLike that accepts address and uint256 as parameter to trigger exit operation in a contract._

#### Interface VatLike 
#### Provides three(3) function signatures of hope,suck and live to perform operation on address, uint256 and read a state variable.
#### _hope : function hope is an external function signature that accepts an address as a parameter._
#### _suck : function suck is an external function signature that accepts address from, address to and uint256 amount._
### _live : function live is an external view function signature that returns uint256(amount) as ouput value._
#### Interface TokenLike 
#### Provides a function signature of transferFrom.
#### _transferFrom : function transferFrom is an external function signature thats accepts parameters of address from, address to and unit256 amount which allows transfer of amount between token accounts within a contract and returns true on success and false on failure._
## CONTRACT
```solidity
abstract contract DssVest {}
```
#### Contract DssVest 
### Is an abstract contract in which at least one of its function interface is not implemented and it is marked with the keyword “abstract”.

## Mapping 
#### Is a reference type variable.
## Public 
#### It is a state visibility that a function or state variable can be access within and outside its existing contract.
```solidity
mapping (address => uint256) public wards;
```
#### wards : Is a mapping with a visibility of public, key type of address and a value type of uint256,located at position 0 and occupies a 32bytes storage slot.
```solidity
 mapping (uint256 => Award) public awards;
```
#### awards : Is a mapping with a visibility of public , key type of uint256 and value type of struct (Award), located at postion 2 and occupies a 32bytes storage slot.
## Struct
```solidity
     struct Award {
        address usr;   // Vesting recipient
        uint48  bgn;   // Start of vesting period  [timestamp]
        uint48  clf;   // The cliff date           [timestamp]
        uint48  fin;   // End of vesting period    [timestamp]
        address mgr;   // A manager address that can yank
        uint8   res;   // Restricted
        uint128 tot;   // Total reward amount
        uint128 rxd;   // Amount of vest claimed
    }
```
#### Award : Is a struct located at position 1, occupies 3 storage slot and contains the following state variables
* _address usr : as vesting recipient and on the first storage slot._
* _uint48 bgn : as the start vesting period and on the first storage slot._
* _uint48 clf : as the cliff date and on the first storage slot._
* _uint48 fin : as the end of vesting period and on the 2nd storage slot._
* _address mgr : as the manager address that can control the contract and on the 2nd storage slot._
* _uint8 res : as the restriction variable and on the 2nd storage slot._
* _uint128 tot : as the total reward amount from the vest and on the 3rd storage slot._
* _uint128 rxd : as the amount of vest claimed and on the 3rd storage slot._

## State Variables 
#### are value type data types declared as global variables with in a contract which can have state visibility of either public, internal or private.
```solidity
    uint256 public cap; 

    uint256 public ids; 
    uint256 internal locked;

    uint256 public constant  TWENTY_YEARS = 20 * 365 days;
```
## Internal
#### It is a state visibility that a function or state variable can be access only within its existing contract.
## Constant 
#### are state variables initialized at compliation time and copied to every location of the state variable. It is used to save gas.
#### _Cap : a value type uint256 state variable, located at postion 3 and occupies 1 storage slot of 32 bytes. It has state visibility of public and It is the maximum per-second issuance token rate of the contract._
#### _ids : a value type uint256 state variable, located at position 4 and occupies 1 storage slot of 32 bytes. It has state visibility of public and It is the total vestings of the contract._
#### _locked :  a value type uint256 state variable, located at position 5 and occupies 1 storage slot of 32 bytes. It has state visibility of internal._
#### _TWENTY_YEARS :  a constant value type uint256 state variable, located at position 6 and occupies 1 storage slot of 32 bytes. It has state visibility of public and it is the time period in which the vesting will last._
## Events 
#### It is used to allow logging to the ethereum blockchain.
```solidity
    event Rely(address indexed usr);
    event Deny(address indexed usr);

    event File(bytes32 indexed what, uint256 data);

    event Init(uint256 indexed id, address indexed usr);
    event Vest(uint256 indexed id, uint256 amt);
    event Restrict(uint256 indexed id);
    event Unrestrict(uint256 indexed id);
    event Yank(uint256 indexed id, uint256 end);
    event Move(uint256 indexed id, address indexed dst);
```
- Event Rely : To emit/log addresses.
- Event Deny : To emit addresses.
- Event File : To emit byte32 and uint256 values.
- Event Init : To emit uint256 and address values.
- Event Vest : To emit uint256 values.
- Event Restrict : To log uint256 values.
- Event Unrestrict : To log uint256 values.
- Event Yank : To log uint256 values.
- Event Move : To log uint256 values.

## Constructor 
```solidity
   constructor() public {
        wards[msg.sender] = 1;
        emit Rely(msg.sender);
    }
```
#### Initialized the msg.sender as the owner at construction time and emit the address of the msg.sender using the “Event Rely”.
## MODIFIER
```solidity
    modifier lock {
        require(locked == 0, "DssVest/system-locked");
        locked = 1;
        _;
        locked = 0;
    }

    modifier auth {
        require(wards[msg.sender] == 1, "DssVest/not-authorized");
        _;
    }
```
#### lock : A modifier to lock the vest.
    - It checks that  “locked” is false with “require” keyword. 
    - sets “locked” to true in order to lock the function.
    - “_;” allows for the execution of other codes of the function and then set “locked” to false.
#### auth : A modifier to check that msg.sender is owner of the contract.
    - It checks that  “msg.sender” is owner with  “require” keyword. 
    - “_;” allows for the execution of other codes of the function.

## FUNCTIONS
```solidity
function usr(uint256 _id) external view returns (address) {
        return awards[_id].usr;
    }

    function bgn(uint256 _id) external view returns (uint256) {
        return awards[_id].bgn;
    }

    function clf(uint256 _id) external view returns (uint256) {
        return awards[_id].clf;
    }

    function fin(uint256 _id) external view returns (uint256) {
        return awards[_id].fin;
    }

    function mgr(uint256 _id) external view returns (address) {
        return awards[_id].mgr;
    }

    function res(uint256 _id) external view returns (uint256) {
        return awards[_id].res;
    }

    function tot(uint256 _id) external view returns (uint256) {
        return awards[_id].tot;
    }

    function rxd(uint256 _id) external view returns (uint256) {
        return awards[_id].rxd;
    }
```
#### usr : Is an external view function that accepts uint256 value as a parameter and returns vesting recipient address as output, using the mapping “awards” to access the struct “Award” value with its “id”.
#### bgn : Is an external view function that accepts uint256 value as a parameter and returns a uint256 start of vesting period as output, using the mapping “awards” to access the struct “Award” value with its “id”.
#### clf : Is an external view function that accepts uint256 value as a parameter and returns a uint256 cliff date as output, using the mapping “awards” to access the struct “Award” value with its “id”.
#### fin : Is an external view function that accepts uint256 value as a parameter and returns a uint256 end of vesting period as output, using the mapping “awards” to access the struct “Award” value with its “id”.
#### mgr : Is an external view function that accepts uint256 value as a parameter and returns an address that controls the vesting as output, using the mapping “awards” to access the struct “Award” value with its “id”.
#### res : Is an external view function that accepts uint256 value as a parameter and returns a uint256 value of the restriction placed on the vesting as output, using the mapping “awards” to access the struct “Award” value with its “id”.
#### tot : Is an external view function that accepts uint256 value as a parameter and returns uint256 total amount as output, using the mapping “awards” to access the struct “Award” value with its “id”.
#### rxd : Is a view function that accepts uint256 value as a parameter and returns a unit256 of amount of vest claimed, using the mapping “awards” to access the struct “Award” value with its “id”.
```solidity
    function rely(address _usr) external auth {
        wards[_usr] = 1;
        emit Rely(_usr);
    }

    function deny(address _usr) external auth {
        wards[_usr] = 0;
        emit Deny(_usr);
    }

    function file(bytes32 what, uint256 data) external lock auth {
        if      (what == "cap")         cap = data;   
        else revert("DssVest/file-unrecognized-param");
        emit File(what, data);
    }
```
#### rely : It is an external function.
    * It authentic the user’s address as a registered address on the contract
    * It can only be trigger by contract owner.
    * It emits user’s address
#### deny : It is an external function.
    * It blacklist user’s address.
    * It can only be trigger by contract owner.
    * It emits user’s address.

#### file : It is an external function that sets the per second issuance rate of the vest contract.
    - It is accepts two parameters of bytes32 and uint256.
    - It emits File event.

### Pure Function : It is a function that neither modifies or read storage state. 
```solidity
 function min(uint256 x, uint256 y) internal pure returns (uint256 z) {
        z = x > y ? y : x;
    }
    function add(uint256 x, uint256 y) internal pure returns (uint256 z) {
        require((z = x + y) >= x, "DssVest/add-overflow");
    }
    function sub(uint256 x, uint256 y) internal pure returns (uint256 z) {
        require((z = x - y) <= x, "DssVest/sub-underflow");
    }
    function mul(uint256 x, uint256 y) internal pure returns (uint256 z) {
        require(y == 0 || (z = x * y) / y == x, "DssVest/mul-overflow");
    }
    function toUint48(uint256 x) internal pure returns (uint48 z) {
        require((z = uint48(x)) == x, "DssVest/uint48-overflow");
    }
    function toUint128(uint256 x) internal pure returns (uint128 z) {
        require((z = uint128(x)) == x, "DssVest/uint128-overflow");
    }
```
#### min : It is an internal pure function that returns uint256.
    - It accepts two uint256 parameters as input.
    - It returns the value of the second parameter if it is less than the first parameter else it returns the first parameter.
#### add : It is an internal pure funtion that returns uint256 value.
    * It accepts two uint256 parameters as input.
    * It adds the two parameters and compare if it is greater or equal to the first parameter.
#### sub: It is an internal pure funtion that returns uint256 value.

    * It accepts two uint256 parameters as input.
    * It subtracts the two parameters and compare if it is less or equal to the first parameter.

#### mul : It is an internal pure funtion that returns uint256 value.

    * It accepts two uint256 parameters as input.
    * It  returns the product of the parameter.

#### touint48 : It is an internal pure funtion that returns uint256 value.

    * It accepts two uint256 parameters as input.
    * It type cast the uint256 parameter to uint48.

#### touint128 : It is an internal pure funtion that returns uint256 value.

    * It accepts two uint256 parameters as input.

    - It type cast the uint256 parameter to uint128.

#### vest : an external function for claiming rewards with user’s id.

#### vest : an external function overload for claming rewards with user’s id and maximum amount.

#### _vest : An internal function using lock modifier to lock the function, 

    - It access the struct “Award” details with mapping “awards” and its input argument “id” .
    - It checks that user’s address is not address zero.
    - It checks that msg.sender is the resgistered address on the contract.
    - It calls “unpaid function”  to get the unpaid tokens of the address(msg.sender).
    - It calls “min function” which returns maxamt if amt is less than maxamount.
    - It checks that the unpaid token is not greater than balance of the address token.
    - It trigger the “pay function” to excute withdraw of unpaid tokens to the user’s address.

#### create : It is a function used to add the details of a new user in the contract with the following checks on the parameters.
```solidity
   function create(address _usr, uint256 _tot, uint256 _bgn, uint256 _tau, uint256 _eta, address _mgr) external lock auth returns (uint256 id) {
        require(_usr != address(0),                        "DssVest/invalid-user");
        require(_tot > 0,                                  "DssVest/no-vest-total-amount");
        require(_bgn < add(block.timestamp, TWENTY_YEARS), "DssVest/bgn-too-far");
        require(_bgn > sub(block.timestamp, TWENTY_YEARS), "DssVest/bgn-too-long-ago");
        require(_tau > 0,                                  "DssVest/tau-zero");
        require(_tot / _tau <= cap,                        "DssVest/rate-too-high");
        require(_tau <= TWENTY_YEARS,                      "DssVest/tau-too-long");
        require(_eta <= _tau,                              "DssVest/eta-too-long");
        require(ids < type(uint256).max,                   "DssVest/ids-overflow");

        id = ++ids;
        awards[id] = Award({
            usr: _usr,
            bgn: toUint48(_bgn),
            clf: toUint48(add(_bgn, _eta)),
            fin: toUint48(add(_bgn, _tau)),
            tot: toUint128(_tot),
            rxd: 0,
            mgr: _mgr,
            res: 0
        });
        emit Init(id, _usr);
    }
```
    - It checks that user address is not address zero.
    - It checks that “_tot” is greater than zero.
    - It checks that “bgn” is greater than the addition of “block.timestamp” and the statevariable  “TWENTY_YEARS”.
    - It checks that “bgn” is greater than the subtraction of “block.timestamp” and the statevariable  “TWENTY_YEARS”.
    - It checks that “_tau” is greater than zero.
    - It checks that the division of “_tot” and “_tau” is less or equal to the cap (maximum per-second issuance token rate).
    - It checks that “_eta” is less or equal to the “_tau”.
    - It checkks that “ids” is less than the maximum of uint256.
    - It pre increase the id count.
    - It creates a new user details with the struct “Award” values by accessing it with mapping “award”.
    - It emits the “Init” event.





