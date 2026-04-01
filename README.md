# English-Auction-with-Time-Extension-Anti-Sniping-
English Auction with Time Extension (Anti‑Sniping)
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract AntiSnipeAuction {
    uint256 public endTime;
    uint256 public highestBid;
    address public highestBidder;

    constructor(uint256 duration) {
        endTime = block.timestamp + duration;
    }

    function bid() public payable {
        require(block.timestamp < endTime, "Ended");
        require(msg.value > highestBid, "Low");

        if (endTime - block.timestamp < 60) {
            endTime += 60; // extend 1 minute
        }

        payable(highestBidder).transfer(highestBid);
        highestBid = msg.value;
        highestBidder = msg.sender;
    }
}
