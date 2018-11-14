<code>Patrick's\
<b>CypherPoker.JS Dev Log</b></code>

<details open>
<summary>Wednesday, November 14, 2018</summary>

One of the surprising problems addressed as part of the fix for issues #4 and #7 was that the EventDispatcher was not differentiating between functions in unique instances of classes (such as CypherPokerContract). As a result, events dispatched for an old instance (on game restart) would be received by a new one and vice versa. This may still be a problem moving forward and will need to be examined in greater detail.

The next issue to be looked at was the contract data comparison within the CypherPokerContract class. One of the major causes of this was the inclusion of internal object properties such as the standard <code>toString</code> function which would not appear in contract data returned from the server. Because the server and local contract structures didn't match, further actions wouldn't be processed (as expected). Changes were made to bypass any <code>function</code> types during property comparison.

Following this, I noted that event listeners in the CypherPokerContract class were all being added and removed together -- both game and P2P message listeners. While game event listeners were being handled at the correct time, the contract still needed to listen for "update" messages in order to fully complete. These two listener types were split up and inserted at the appropriate points in the code.

In addition to this I observed that the server was registering contracts under incorrect dealers on subsequent rounds. After closer examination it became clear that this was happening because the associated table information was not being updated. While this was not an issue in the client, the server used the table information to determine player roles such as the dealer so on a restart (with a new dealer), the server would mis-register the contract to the old dealer. This has been updated and roles now appear to be handled correctly.

The previous problem also appeared to a lesser degree in the client because data such as the players array and table object were not isolated between the contract and the game. In other words, the contract was using some of the game's data even after the game had ended and that data was subsequently updated (i.e. no longer correct for the contract). This data was added as internal copies within the CypherPokerContract instance so that it can complete in the background while a new hand (with updated data) begins.

The game is now able to complete a full game (hand), but is not continuing past the point where a new contract is registered by the next dealer--no "agree" action is being processed by the other players. This is where I'm currently focusing my attention and I'm expecting subsequent actions to work correctly if this initial action can be successfully processed.
</details>
