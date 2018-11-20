<details open>
<summary>Tuesday, November 20, 2018</summary>
<br/>
I've confirmed that issue <a href="https://github.com/monicanagent/cypherpoker.js/issues/4">4</a> no longer appears and have also fixed the problem where contracts won't restart on subsequent hands--with <a href="https://github.com/monicanagent/cypherpoker.js/issues/9">one exception</a>: in a multi-player game (3+), if one or more players fold in a subsequent hand the post-game analysis doesn't kick off (i.e. the contract doesn't complete). This doesn't occur if all but one players have folded; in other words, at least two players must play the hand to the end.<br/>
<br/>
This is most likely a similar issue to those have been plaguing the other contract restarts but on a positive note it appears to be one of the last major issues related to contract handling.<br/>
<br/>
There is <a href="https://github.com/monicanagent/cypherpoker.js/issues/8">another contract-based issue</a> in which multiple winning hands are returned in a post-game analysis if the highest winning hand was a high-card but the payout appears to be handled correctly so it's a minor issue.<br/>
<br/>
Finally, the <a href="https://github.com/monicanagent/cypherpoker.js/issues/7">player timeout problem</a> is still lingering but it takes a lesser precedence than the other problems above since a timeout on a successfully completed contract simply results in an "invalid contract" error.<br/>
<br/>
Work continues...
</details>
<br/>
<details>
<summary>Monday, November 19, 2018</summary>
<br/>
I worked through the weekend but unfortunately I wasn't able to address all of the issues that have come up.<br/>
<br/>
The <a href="https://github.com/monicanagent/cypherpoker.js/issues/4">duplicate card selection issue</a> ended up being a simple fix with a surprisingly unintuitive source: card selections from other players were being <a href="https://github.com/monicanagent/cypherpoker.js/blob/485c7639e820d1ee6db5de5d825d690a09640b2b/src/web/scripts/CypherPokerGame.js#L2096">excluded from the selection process if they weren't private</a>. How does excluding cards duplicate them in the contract? Simply because if a card being selected was public it wouldn't be removed from the face-down deck, thereby allowing it to <i>sometimes</i> be selected twice.<br/>
<br/>
I haven't logged the other issues I've encountered during my testing but they all have to do with game restarts/ends and specifically to the contract. Although games are now successfully ending and restarting on the first try, the second round causes various validation errors on the server-side contract (though the contracts are mostly completing).<br/>
<br/>
Initially I was suspicious that perhaps the validation process had some errors but it turns out that every problem being reported is indeed a failure. For example, in subsequent (after the first game/hand) rounds of deck encryption, the first encryption by the dealer is not being stored to the contract. It took some digging and comparing raw data between the <a href="https://github.com/monicanagent/cypherpoker.js/blob/master/src/server/api/CP_SmartContract.js">server contract</a> and the <a href="https://github.com/monicanagent/cypherpoker.js/blob/master/src/web/scripts/CypherPokerAnalyzer.js">CypherPokerAnalyzer</a> data, which is correct, to figure this out.<br/>
<br/>
Although I'll probably need another day or two to knock of the rest of the issues, I am making incremental progress and am seeing definitive patterns emerge as to where the bugs are and what their likely source is. This is a good thing considering the asynchronous complexity of the software.<br/>
<br/>
I'm not sure if this will put much of a dent in the v0.3.0 update schedule--for now I'm still optimistic that it won't.
</details>

<br/>

<details>
<summary>Friday, November 16, 2018</summary>
<br/>
It appears that issue <a href="https://github.com/monicanagent/cypherpoker.js/issues/4">#4</a> is now fixed; I only need to test more extensively with different combinations of players and timings (restart immediately at end of game, restart after brief delay, restart with/without switching browser tabs, etc.)<br/>
<br/>
Issue <a href="https://github.com/monicanagent/cypherpoker.js/issues/7">#7</a> (player timeout continues beyond the end of the game), is still appearing but I should also be able to address it shortly as there are only a limited number of possible causes for it: a "gameend" event is not being dispatched (unlikely), the listeners for the event are being removed before being dispatched (more likely), an unhandled exception is being thrown during the processing of the event (also quite likely).<br/>
<br/>
I've noted that in some instances it's only the user interface that's reporting the timeout (it's not being reported to the contract), which is enouraging because it suggests that the issue may be limited mostly to the <a href="https://github.com/monicanagent/cypherpoker.js/blob/master/src/web/scripts/CypherPokerUI.js">CypherPokerUI</a> class.<br/>
<br/>
I was hoping to have these fixes already in place today but I'm quite close and should have them in place by the end of this weekend which means I'll be on track for v0.3.0 (the user interface upgrade).
</details>

<br/>

<details>
<summary>Thursday, November 15, 2018</summary>
<br/>
Today I'm addressing the inverse of yesterday's issue; namely, the certain data structures such as the players array are captured <i>too</i> early in the creation of a contract when restarting the game.<br/>
<br/>
Essentially what happens is that information such as this array, along with included data such as the keychains, is captured in a new <a href="https://github.com/monicanagent/cypherpoker.js/blob/master/src/web/scripts/CypherPokerContract.js">CypherPokerContract</a> instance before the data is generated. This causes a mismatch between the received contract, as generated by the dealer, and the local (in-game) data when comparing values like the shared prime modulus.<br/>
<br/>
It looks like I'm the right track to resolving this issue and any similar issues that might appear subsequently. After these fixes are in place I'll re-enable the <a href="https://github.com/monicanagent/cypherpoker.js/blob/master/src/web/scripts/CypherPokerAnalyzer.js">CypherPokerAnalyzer</a>, which is currently disabled (to minimize any additional post-game issues), and with that version 0.2.3 will be ready for the UI updates slated for version 0.3.0
</details>

<br/>

<details>
<summary>Wednesday, November 14, 2018</summary>
<br/>
One of the surprising problems addressed as part of the fix for issues #4 and #7 was that the EventDispatcher was not differentiating between functions in unique instances of classes (such as CypherPokerContract). As a result, events dispatched for an old instance (on game restart) would be received by a new one and vice versa. This may still be a problem moving forward and will need to be examined in greater detail.<br/>
<br/>
The next issue to be looked at was the contract data comparison within the CypherPokerContract class. One of the major causes of this was the inclusion of internal object properties such as the standard <code>toString</code> function which would not appear in contract data returned from the server. Because the server and local contract structures didn't match, further actions wouldn't be processed (as expected). Changes were made to bypass any <code>function</code> types during property comparison.<br/>
<br/>
Following this, I noted that event listeners in the CypherPokerContract class were all being added and removed together -- both game and P2P message listeners. While game event listeners were being handled at the correct time, the contract still needed to listen for "update" messages in order to fully complete. These two listener types were split up and inserted at the appropriate points in the code.<br/>
<br/>
In addition to this I observed that the server was registering contracts under incorrect dealers on subsequent rounds. After closer examination it became clear that this was happening because the associated table information was not being updated. While this was not an issue in the client, the server used the table information to determine player roles such as the dealer so on a restart (with a new dealer), the server would mis-register the contract to the old dealer. This has been updated and roles now appear to be handled correctly.<br/>
<br/>
The previous problem also appeared to a lesser degree in the client because data such as the players array and table object were not isolated between the contract and the game. In other words, the contract was using some of the game's data even after the game had ended and that data was subsequently updated (i.e. no longer correct for the contract). This data was added as internal copies within the CypherPokerContract instance so that it can complete in the background while a new hand (with updated data) begins.<br/>
<br/>
The game is now able to complete a full game (hand), but is not continuing past the point where a new contract is registered by the next dealer--no "agree" action is being processed by the other players. This is where I'm currently focusing my attention and I'm expecting subsequent actions to work correctly if this initial action can be successfully processed.
</details>
