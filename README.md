# Experiment 3: Blockchain-Based Crowdfunding (Kickstarter Alternative)
```
Name: DHINESH R
Reg no: 212223220019
```

## Aim:
To create a decentralized crowdfunding platform where donors contribute funds only if the campaign goal is met.

## Algorithm:
A project owner starts a campaign with a funding goal and deadline.


Contributors can send ETH to the campaign.


If the goal is met before the deadline, funds are released to the project owner.


If the goal is not met, contributors can withdraw their funds.


## Program:
```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract Crowdfunding {
    struct Campaign {
        address creator;
        uint256 goal;
        uint256 deadline;
        uint256 amountRaised;
        bool goalMet;
        mapping(address => uint256) contributions;
    }

    Campaign public campaign;

    constructor(uint256 _goal, uint256 _duration) {
        campaign.creator = msg.sender;
        campaign.goal = _goal;
        campaign.deadline = block.timestamp + _duration;
    }

    function contribute() public payable {
        require(block.timestamp < campaign.deadline, "Campaign ended");
        campaign.amountRaised += msg.value;
        campaign.contributions[msg.sender] += msg.value;
    }

    function withdrawFunds() public {
        require(msg.sender == campaign.creator, "Only creator can withdraw");
        require(campaign.amountRaised >= campaign.goal, "Goal not met");
        payable(msg.sender).transfer(campaign.amountRaised);
        campaign.goalMet = true;
    }

    function refund() public {
        require(block.timestamp > campaign.deadline, "Campaign still active");
        require(campaign.amountRaised < campaign.goal, "Goal was met");
        uint256 amount = campaign.contributions[msg.sender];
        campaign.contributions[msg.sender] = 0;
        payable(msg.sender).transfer(amount);
    }
}
```
# Expected Output:
Users can contribute ETH to the campaign.
<img width="1920" height="1128" alt="Screenshot 2026-08-14 135134" src="https://github.com/user-attachments/assets/ac2fce05-e574-444c-8d48-c15a03dcf373" />




If the goal is met, the creator can withdraw funds.
<img width="1920" height="1128" alt="Screenshot 2026-08-14 135218" src="https://github.com/user-attachments/assets/4f503b2f-eee8-4941-9696-fcf408965596" />




If the goal is not met, contributors can claim a refund.
<img width="1920" height="1128" alt="Screenshot 2026-08-14 135228" src="https://github.com/user-attachments/assets/7ad97556-c170-441d-8179-826993f20a90" />




# High-Level Overview:
Teaches decentralized fundraising.


Avoids fraud by ensuring funds are only transferred if the goal is met.

# RESULT: 
Thus, a decentralized crowdfunding platform has been created and successfully executed.
