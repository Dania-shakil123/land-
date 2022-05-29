//SPDX-License-Identifier: Unlicense
pragma solidity ^0.8.10;

contract Land {

    //Step 1: (Create LandRegistry)
    struct LandReg {
        uint landId;
        uint area;
        string city;
        string state;
        uint landPrice;
        uint propertyPID;
    }

    //Step 2: (Buyer details)
    struct Buyer{
        address id;
        string name;
        uint age;
        string city;
        string cnic;
        string email;
    }

    //Step 3: (Seller details)
    struct Seller{
        address id;
        string name;
        uint age;
        string city;
        string cnic;
        string email;
    }

    //Step 4: (LandInspector details)
    struct LandInspector {
        address id;
        string name;
        uint age;
        string designation;
    }
// for request access 
    struct LandRequest{
        uint reqId;
        address sellerId;
        address buyerId;
        uint landId;
    }

    //mappings
    mapping(uint => LandReg) public lands;
    mapping(address => LandInspector) public inspectormapping;
    mapping(address => Seller) public sellerMapping;
    mapping(address => Buyer) public buyermapping;
    mapping(uint => LandRequest) public requestsList;

    mapping(address => bool) public registerAddress;
    mapping(address => bool) public registeredSeller;
    mapping(address => bool) public registeredBuyer;
    mapping(address => bool) public verifiedSeller;
    mapping(address => bool) public verifiedBuyer;
    mapping(uint => bool) public verfiedLand;
    mapping(uint => address) public landOwner;
    mapping(uint => bool) public requestStatus;
    mapping(uint => bool) public landsRequested;

    //to store the address of deployer (landInspector)
    address public landInspector;
    address[] public sellers;
    address[] public buyers;

    uint public landsCount;
    uint public inspectorsCount;
    uint public sellersCount;
    uint public buyersCount;
    uint public requestsCount;

    constructor(string memory _name, uint _age, string memory _designation){
        landInspector = msg.sender ;
        addLandInspector(msg.sender, _name, _age, _designation);
    }
//for step 9
    modifier onlyLandInspector {
      require(msg.sender == landInspector);
      _;
    }

    modifier isSeller(address _id) {
      require(registeredSeller[_id]);
      _;
    }
    modifier isVerifiedSeller(address _id){
      require(verifiedSeller[_id]);
      _;
    }

    modifier isBuyer(address _id) {
      require(registeredBuyer[_id]);
      _;
    }
    modifier isVerifiedBuyer(address _id){
      require(verifiedBuyer[_id]);
      _;
    }
//1st address
    function addLandInspector(address _address, string memory _name, uint _age, string memory _designation) private {
        inspectorsCount++;
        inspectormapping[_address] = LandInspector(_address, _name, _age, _designation);
    }

    function getLandInspector(address _inspectorAddress) public view returns (LandInspector memory) {
        return inspectormapping[_inspectorAddress];
    }

    function getLandsCount() public view returns (uint) {
        return landsCount;
    }

    function getBuyersCount() public view returns (uint) {
        return buyersCount;
    }

    function getSellersCount() public view returns (uint) {
        return sellersCount;
    }

    function getRequestsCount() public view returns (uint) {
        return requestsCount;
    }
    function getArea(uint i) public view returns (uint) {
        return lands[i].area;
    }
    //get city
    function getLandCity(uint i) public view returns (string memory) {
        return lands[i].city;
    }
     function getState(uint i) public view returns (string memory) {
        return lands[i].state;
    }

    function getLandPrice(uint i) public view returns (uint) {
        return lands[i].landPrice;
    }
    function getPID(uint i) public view returns (uint) {
        return lands[i].propertyPID;
    }
      //step 13, 18 
    //get owner of landId    
    function getlandOwner(uint _landId) public view returns (address) {
        return landOwner[_landId];
    }
     //step 7 seller address copy , inspector address select
    function IsSeller(address _sellerId) public onlyLandInspector{
        verifiedSeller[_sellerId] = true;
    }
     //step 15 
    function IsBuyer(address _buyerId) public onlyLandInspector{
        verifiedBuyer[_buyerId] = true;
    }
     //step 9
    function verifyLand(uint _landId) public onlyLandInspector{
        verfiedLand[_landId] = true;
    }

    function LandisVerified(uint _id) public view returns (bool) {
        if(verfiedLand[_id])
            return true;
        return false;
    }

    function isRegistered(address _id) public view returns (bool) {
        if(registerAddress[_id])
            return true;
        return false;
    }
     //step 8 on seller address 
    function addLand(uint _area, string memory _city,string memory _state, uint landPrice, uint _propertyPID) public isSeller(msg.sender) isVerifiedSeller(msg.sender) {
        landsCount++;
        lands[landsCount] = LandReg(landsCount, _area, _city, _state, landPrice,_propertyPID);
        landOwner[landsCount] = msg.sender;
    }
    //step 12
    function getLandDetails(uint _landId) public view returns (LandReg memory) {
        return lands[_landId];
    }
     //step 6
    // address 2nd (seller)
    //registration of seller
    function registerSeller(string memory _name, uint _age, string memory _city, string memory _cnic, string memory _email) public {
        //require that Seller is not already registered
        require(!registerAddress[msg.sender]);

        registerAddress[msg.sender] = true;
        registeredSeller[msg.sender] = true ;
        sellersCount++;
        sellerMapping[msg.sender] = Seller(msg.sender, _name, _age, _city, _cnic,_email);
        sellers.push(msg.sender);
    }
    //step 10
    function updateSeller(string memory _name, uint _age, string memory _city, string memory _cnic, string memory _email) public {
        //require that Seller is already registered
        require(registerAddress[msg.sender] && (sellerMapping[msg.sender].id == msg.sender));
        sellerMapping[msg.sender].name = _name;
        sellerMapping[msg.sender].age = _age;
        sellerMapping[msg.sender].city = _city;
        sellerMapping[msg.sender].cnic = _cnic;
        sellerMapping[msg.sender].email = _email;
    }

    function getSeller() public view returns( address [] memory ){
        return(sellers);
    }

    function getSellerDetails(address i) public view returns (string memory, uint, string memory, string memory) {
        return (sellerMapping[i].name, sellerMapping[i].age, sellerMapping[i].cnic, sellerMapping[i].email);
    }
     //step 11
    function SellerisVerified(address _id) public view returns (bool) {
        if(verifiedSeller[_id])
            return true;
        return false;
    }

    //step 14
 // 3rd address of buyer 
    function registerBuyer(string memory _name, uint _age, string memory _city, string memory _cnic, string memory _email) public {
        //require that Buyer is not already registered
        require(!registerAddress[msg.sender]);
        registerAddress[msg.sender] = true;
        registeredBuyer[msg.sender] = true ;
        buyersCount++;
        buyermapping[msg.sender] = Buyer(msg.sender, _name, _age, _city, _cnic, _email);
        buyers.push(msg.sender);
    }
   //step 16
    function updateBuyer(string memory _name,uint _age, string memory _city,string memory _cnic, string memory _email) public {
        //require that Buyer is already registered
        require(registerAddress[msg.sender] && (buyermapping[msg.sender].id == msg.sender));
        buyermapping[msg.sender].name = _name;
        buyermapping[msg.sender].age = _age;
        buyermapping[msg.sender].city = _city;
        buyermapping[msg.sender].cnic = _cnic;
        buyermapping[msg.sender].email = _email;
    }
     //step 17
    function BuyerisVerified(address _id) public view returns (bool) {
        if(verifiedBuyer[_id])
            return true;
        return false;
    }

    function getBuyer() public view returns( address [] memory ){
        return(buyers);
    }

    function getBuyerDetails(address i) public view returns ( string memory,string memory, string memory, string memory, uint, string memory) {
        return (buyermapping[i].name,buyermapping[i].city , buyermapping[i].email, buyermapping[i].email, buyermapping[i].age, buyermapping[i].cnic);
    }
//req access page 1
    function requestLand(address _sellerId, uint _landId) public isBuyer(msg.sender) isVerifiedBuyer(msg.sender) {
        requestsCount++;
        requestsList[requestsCount] = LandRequest(requestsCount, _sellerId, msg.sender, _landId);
        requestStatus[requestsCount] = false;
        landsRequested[requestsCount] = true;
    }

    function getRequestDetails (uint i) public view returns (address, address, uint, bool) {
        return(requestsList[i].sellerId, requestsList[i].buyerId, requestsList[i].landId, requestStatus[i]);
    }

    function isRequested(uint _id) public view returns (bool) {
        if(landsRequested[_id])
            return true;
        return false;
    }

    function isApproved(uint _id) public view returns (bool) {
        if(requestStatus[_id])
            return true;
        return false;
    }

    function approveRequest(uint _reqId) public isSeller(msg.sender) isVerifiedSeller(msg.sender) {
        requestStatus[_reqId] = true;
    }
     //step 20,19
    //to buy land on given price and transfer ownership of land to caller
    function buyLand(uint256 _landId) public payable { 
        require (msg.value == getLandPrice(_landId), "price to buy a land must be equal to land price");
        if( verifiedBuyer[msg.sender] && LandisVerified(_landId) ){
          address prevOwner = getlandOwner(_landId);
            landOwner[_landId] = msg.sender;
            
            payable(prevOwner).transfer(msg.value);
        }
    }
     //step 21 
    //to transfer ownership to new owner
    function transferOwnership(uint _landId, address _newOwner) public{
        if (LandisVerified(_landId) && msg.sender == landOwner[_landId] ) {
            landOwner[_landId] = _newOwner;
        }
    }
}
