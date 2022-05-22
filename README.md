# Multi-sig-wallet

pragma solidity ^0.5;
pragma experimental ABIEncoderV2;
contract Wallet {
    address[] public approvers;
    uint public quorum;
    struct Transfer {
        uint id;
        uint amount;
        address payable to;
        uint approvals;
        bool sent;
    }
    Transfer[] public transfers;
    mapping(address => mapping(uint => bool)) public approvals;

    constructor(address[] memory _approvers, uint _quorum) public {
        approvers = _approvers;
        quorum = _quorum;
       }

       function getApprovers() external view returns(address[] memory) {

           return approvers;
       }

       function getTransfers() external view returns(Transfer[] memory) {

           return transfers;

       }

       function createTransfer(uint amount, address payable to) external onlyApprover{
           transfers[nextId] = Transfer(
               nextId,
               amount,
               to,
               0,
               false
           );
           nextId++;
       }
}

        function approveTransfer(uint id) external onlyApprover{
            require{transfers[id].sent == false, 'transfer already sent'}
            require(approvals[msg.sender][id] == false, 'Cannot approve transfer twice');

            approvals[msg.sender][id] = true;
            transfers[id].approvals++;

            if(transfers[id].approvals >= quorum) {
                transfers[id].sent = true;
                address payable to = transfers[id].to;
                uint amount = trasnfers[id].amount;
                to.transfer(amount);
            }
        }

receive() external payable {}
modifer onlyAllower = false;
for(uint i = 0; i< approvers.length; i++) {
    if(approvers[i] == msg.sender) {

        allowed = true;
    }
}

require(allowed == true, 'only approver allowed');
_;
