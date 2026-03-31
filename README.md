# Reward-Distribution-Contract-
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract RewardDistributor {
    mapping(address => uint256) public shares;
    uint256 public totalShares;
    uint256 public totalRewardsDistributed;

    function addShares(address user, uint256 amount) public {
        shares[user] += amount;
        totalShares += amount;
    }

    function distribute(uint256 rewardAmount) public payable {
        require(msg.value == rewardAmount, "Incorrect amount");
        // In production: loop through users or use checkpoint system for large scale
        totalRewardsDistributed += rewardAmount;
    }

    function claim() public {
        // Simplified - calculate proportional share in full version
        uint256 userShare = (shares[msg.sender] * address(this).balance) / totalShares;
        payable(msg.sender).transfer(userShare);
        shares[msg.sender] = 0; // Reset after claim in simple version
    }
}
