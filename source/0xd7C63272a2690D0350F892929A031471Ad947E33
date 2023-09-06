// SPDX-License-Identifier: MIT

pragma solidity ^0.8.7;
contract SCCrowd {
    address public owner; 
    uint public projectTax; 
    uint public projectCount; 
    uint public balance; 
    statsStruct public stats; 
    projectStruct[] projects; 

    mapping (address =>  projectStruct[]) projectOf; 
    mapping (uint => donatorsStruct[]) donatorsOf; 
    mapping (uint  => bool) public projectExists; 
   
    enum statusEnum {
        OPEN,
        APPROVED,
        REVERTED,
        DELETED,
        PAIDOUT
    }

    struct statsStruct {
        uint totalProjects;
        uint totalDonating;
        uint totalDonations;
    }

    struct donatorsStruct {
        address owner;
        uint contribution;
        uint timestamp;
        bool refunded;
    }

    struct projectStruct {
        uint id;
        address owner;
        string title;
        string description;
        string imageURL;
        uint cost;
        uint raised;
        uint timestamp;
        uint expiresAt;
        uint donators;
        statusEnum status;
    }

    modifier ownerOnly(){
        require(msg.sender == owner, "Owner reserved only");
        _;
    }

    event Action (
        uint256 id,
        string actionType,
        address indexed executor,
        uint256 timestamp
    );

    constructor(uint _projectTax) {
        owner = msg.sender;
        projectTax = _projectTax;
    }

    function createProject(
        string memory title,
        string memory description,
        string memory imageURL,
        uint cost,
        uint expiresAt
    ) public returns (bool) {
        require(bytes(title).length > 0, "Title cannot be empty");
        require(bytes(description).length > 0, "Description cannot be empty");
        require(bytes(imageURL).length > 0, "ImageURL cannot be empty");
        require(cost > 0 ether, "Cost cannot be zero");

        projectStruct memory project;
        project.id = projectCount;
        project.owner = msg.sender;
        project.title = title;
        project.description = description;
        project.imageURL = imageURL;
        project.cost = cost;
        project.timestamp = block.timestamp;
        project.expiresAt = expiresAt;

        projects.push(project);
        projectExists[projectCount] = true; 
        projectOf[msg.sender].push(project);
        stats.totalProjects += 1;

        emit Action (
            projectCount++,
            "PROJECT CREATED",
            msg.sender,
            block.timestamp
        );
        return true;
    }

    function updateProject(
        uint id,
        string memory title,
        string memory description,
        string memory imageURL,
        uint expiresAt
    ) public returns (bool) {
        require(msg.sender == projects[id].owner, "Unauthorized Entity");
        require(bytes(title).length > 0, "Title cannot be empty");
        require(bytes(description).length > 0, "Description cannot be empty");
        require(bytes(imageURL).length > 0, "ImageURL cannot be empty");

        projects[id].title = title;
        projects[id].description = description;
        projects[id].imageURL = imageURL;
        projects[id].expiresAt = expiresAt;

        emit Action (
            id,
            "PROJECT UPDATED",
            msg.sender,
            block.timestamp
        );

        return true;
    }

    function deleteProject(uint id) public returns (bool) {
        require(projects[id].status == statusEnum.OPEN, "Project no longer opened");
        require(msg.sender == projects[id].owner, "Unauthorized Entity");

        projects[id].status = statusEnum.DELETED;
        performRefund(id);

        emit Action (
            id,
            "PROJECT DELETED",
            msg.sender,
            block.timestamp
        );

        return true;
    }

    function performRefund(uint id) internal {
        for(uint i = 0; i < donatorsOf[id].length; i++) {
            address _owner = donatorsOf[id][i].owner;
            uint _contribution = donatorsOf[id][i].contribution;
            
            donatorsOf[id][i].refunded = true;
            donatorsOf[id][i].timestamp = block.timestamp;
            payTo(_owner, _contribution);

            stats.totalDonating -= 1;
            stats.totalDonations -= _contribution;
        }
    }

    function donateProject(uint id) public payable returns (bool) {
        require(msg.value > 0 ether, "BNB must be greater than zero");
        require(projectExists[id], "Project not found");
        require(projects[id].status == statusEnum.OPEN, "Project no longer opened");

        stats.totalDonating += 1;
        stats.totalDonations += msg.value;
        projects[id].raised += msg.value;
        projects[id].donators += 1;

        donatorsOf[id].push(
            donatorsStruct(
                msg.sender,
                msg.value,
                block.timestamp,
                false
            )
        );

        emit Action (
            id,
            "PROJECT DONATED",
            msg.sender,
            block.timestamp
        );

        if(projects[id].raised >= projects[id].cost) {
            projects[id].status = statusEnum.APPROVED;
            balance += projects[id].raised;
            performPayout(id);
            return true;
        }

        if(block.timestamp >= projects[id].expiresAt) {
            projects[id].status = statusEnum.REVERTED;
            performRefund(id);
            return true;
        }

        return true;
    }

    function performPayout(uint id) internal {
        uint raised = projects[id].raised;
        uint tax = (raised * projectTax) / 100;

        projects[id].status = statusEnum.PAIDOUT;

        payTo(projects[id].owner, (raised - tax));
        payTo(owner, tax);

        balance -= projects[id].raised;

        emit Action (
            id,
            "PROJECT PAID OUT",
            msg.sender,
            block.timestamp
        );
    }

    function requestRefund(uint id) public returns (bool) {
        require(
            projects[id].status != statusEnum.REVERTED ||
            projects[id].status != statusEnum.DELETED,
            "Project not marked as revert or delete"
        );
        
        projects[id].status = statusEnum.REVERTED;
        performRefund(id);
        return true;
    }

    function payOutProject(uint id) public returns (bool) {
        require(projects[id].status == statusEnum.APPROVED, "Project not APPROVED");
        require(
            msg.sender == projects[id].owner ||
            msg.sender == owner,
            "Unauthorized Entity"
        );

        performPayout(id);
        return true;
    }

    function changeTax(uint _taxPct) public ownerOnly {
        projectTax = _taxPct;
    }

    function getProject(uint id) public view returns (projectStruct memory) {
        require(projectExists[id], "Project not found");

        return projects[id];
    }

     function getProjects() public view returns (projectStruct[] memory) {
        return projects;
    }

    function getDonators(uint id) public view returns (donatorsStruct[] memory) {
        return donatorsOf[id];
    }

   function payTo(address to, uint256 amount) internal {
        (bool success, ) = payable(to).call{value: amount}("");
        require(success);
    }
}