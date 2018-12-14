<details open>
<summary>Friday, December 14, 2018</summary>
<br/>
Version <a href="https://github.com/monicanagent/cypherpoker.js/commit/82baa50646d0b6b7a834411cee29c8611409852f">0.3.0-beta.1</a> has now been committed to the repository and the <a href="https://monicanagent.github.io/cypherpoker.js/demo/web/demo">demo is live</a>!<br/>
<br/>
I <i>had</i> wanted it live earlier but once I got the code wrinkles worked out there turned out to be problems with the server hosting. Still, better late than never :)<br/>
<br/>
With this release there are many obvious user interface changes including colour scheme and style updates, a new font, and help sections for nearly everything that's visible.<br/>
<br/>
There are also some minor functional fixes, one of the major ones being not allowing players to bet more than their available in-game balance.<br/>
<br/>
After being looked at by someone other than myself it looks like the UI isn't as self-explanatory as I'd hoped so I'll be making some content changes this weekend. After that, over the next few weeks, I'll be making various live changes to fix any major issues and fill in some of the missing gaps (<a href="https://github.com/monicanagent/cypherpoker.js/milestone/3">version 0.3.1</a>).<br/>
<br/>
This will include adding some sort of "main menu" system so that players can get back to the account management functions as well as being able to return to the table creation / join interfaces. Furthermore, I'm also going to be testing out multiple concurrent tables (up until now I've been testing almost exclusively with one table at a time), updating the "join table" button interface to make it more prominent, adding a customizable player timeout (right now it defaults to 20 seconds), and adding an alias input option for players joining a table.<br/>
<br/>
I don't expect these to require any major coding efforts but some new functionality will need to be built, especially for the "main menu" portion.<br/>
<br/>
If I notice any other bugs or useful features, I'll add them to the 0.3.1 milestone and any other major inclusions will probably be scheduled for version 0.3.2<br/>
<br/>
In any event, v0.3.0 marks a major milestone--a playable, mostly complete, reliable public demo that answers the question: "but can I play it?"<br/>
<br/>
</details>
<br/>
<details>
<summary>Monday, December 10, 2018</summary>
<br/>
The final touches are now being added to the v0.3.0 beta!<br/>
<br/>
The changes I ended up making were a bit more extensive than I'd initially planned but I'm quite pleased with the results.<br/>
<br/>
In a nutshell, the application now builds itself out of standalone external HTML snippets called templates which are added and removed to/from the page dynamically during runtime.<br/>
<br/>
This was done to make the content more manageable (as opposed to a single, flat HTML file), as well as making it much easier to insert metadata (replacing special tags in the HTML with dynamic data).<br/>
<br/>
One of the interesting results of this update has been that the <a href="https://github.com/monicanagent/cypherpoker.js/blob/master/src/web/scripts/CypherPokerUI.js">CypherPokerUI</a> class has gotten significantly smaller while the complexity has increased. Neat.<br/>
<br/>
I'm hoping to have v0.3.0-beta online later tonight or tomorrow.<br/>
<br/>
</details>
<br/>
<details>
<summary>Monday, December 3, 2018</summary>
<br/>
Looks like I missed that end-of-week deadline for <a href="https://github.com/monicanagent/cypherpoker.js/milestone/2">v0.3.0</a> but I'm very happy that I managed to address those <a href="https://github.com/monicanagent/cypherpoker.js/commit/86d03b0637e7ae1e4601545971d64881c8f7fbbc">two outstanding issues</a>. Game play now appears to function correctly for any number of players. This includes restarts (multiple hands/games), post-game verification/validation, timeout/penalty/winning handling, and of course all of the existing cryptocurrency functionality such as deposits, transfers, and cashouts.<br/>
<br/>
Regarding the latest fixes, the in-between hands/games state is a really tricky one because it has to be able to accept a new game in the same instance while completing a potentially unresolved one in the background, especially with regard to the smart contract. I could've cheaped out here and just ignored potential lags in order to get it done quicker but I'm pretty sure it would've come back to bite me in the ass in the future ... right about the time I involved actual smart contracts.<br/>
<br/>
Anyways, I've extended the v0.3.0 due date by one more week in order to <a href="https://github.com/monicanagent/cypherpoker.js/issues/11">implement the user interface changes</a>. I don't expect too many bugs to come out of this as I'll mostly be shifting around HTML containers, updating stylesheets, and making minor modifications to the <a href="https://github.com/monicanagent/cypherpoker.js/blob/master/src/web/scripts/CypherPokerUI.js">CypherPokerUI</a> class. In other words, while the interaction flow will be different, there should be no significant logic changes to the core game code. By this time next week we should be looking at a fully-functional, debugged, and user-friendly beta!<br/>
<br/>
</details>
<br/>
<details>
<summary>Thursday, November 29, 2018</summary>
<br/>
There was little to update yesterday as I made little progress but today, at last, I made a breakthrough on the final major issue for v0.3.0.<br/>
<br/>
The <a href="https://github.com/monicanagent/cypherpoker.js/issues/10#issuecomment-442983023">issue notes</a> have been updated with my findings and at this point the problem can be considered fixed although its root remains: there are two key objects being used for analysis instead of the expected one.<br/>
<br/>
The current fix involves simply using the key object at the highest index (in the chain), but it's not a great fix since in the future the plan is to use (potentially) multiple keys in a hand/game. In other words, where only one key object is currently expected, in the future there could be more than one.<br/>
<br/>
In any event, the end is clearly in sight now and in the worst-case scenario I can simply upload the current fix an flag it for a future update (before multiple keys per hand are used).<br/>
<br/>
</details>
<br/>
<details>
<summary>Tuesday, November 27, 2018</summary>
<br/>
There wasn't much time to get in front of a keyboard today so there's nothing new to update. However, I did give some thoughts to the remaining issues and the new design, which I'll hopefully be able to share soon.<br/>
<br/>
</details>
<br/>
<details>
<summary>Monday, November 26, 2018</summary>
<br/>
I managed to re-enable the post-game <a href="https://github.com/monicanagent/cypherpoker.js/blob/master/src/web/scripts/CypherPokerAnalyzer.js">analyzer</a>, the part of the code the verifies the cryptographic correctness and winning hand(s) in the browser (the <a href="https://github.com/monicanagent/cypherpoker.js/blob/master/src/server/api/CP_SmartContract.js">server contract</a> is expected to contain similar functionality).<br/>
<br/>
It works, so long as the next dealer doesn't hit the "RESTART" button for the table before the analyzer has a chance to spit out a final analysis (did the game verify correctly cryptographically? did the game adhere to established rules? who had the best / winning hands? what were everyone's best hands? etc). In that very possible condition, the analysis simply stops and the hand results fails to show up in the Hand History. It's not fatal--everything else resumes independently and correctly, including the contract--but it's less than ideal.<br/>
<br/>
I <a href="https://github.com/monicanagent/cypherpoker.js/commit/bbedca1bcde4117e6400f5589451c6920ed3cff7">added an update</a> on Saturday afternoon to the repository to address the associated issue and the previous paragraph describes why this is a <i>partial</i> fix.<br/>
<br/>
It's one of those proverbial dark 'n stormy nights here in Toronto, some slow Adele song is on the radio as the cold November rain falls on the pavement, and I think, it's okay to feel a little less than enthusiastic when starting the week like this.<br/>
<br/>
On the bright side, I haven't noticed any new issues which suggests that v0.3.0 is very much within reach. The fact that pieces of the game software can continue to function correctly despite other pieces failing--like the analyzer--also speaks to the strength and versatility of the underlying architecture.<br/>
<br/>
That being said, I guess I should also start doing some UI design work...<br/>
<br/>
</details>
<br/>
<details>
<summary>Friday afternoon, November 23, 2018</summary>
<br/>
I skipped the update last night and decided instead to push on through  the darkness and then back into the bleary light of morning in order to finally fix <a href="https://github.com/monicanagent/cypherpoker.js/milestone/1">those critical issues that were plaguing version 0.2.3</a><br/>
<br/>
There are presently <a href="https://github.com/monicanagent/cypherpoker.js/milestones/v0.3.0">two remaining bugs</a> that have been added to the v0.3.0 release (one of them is new), neither of which are fatal or cause malfunctions so some of the user interface work can be done alongside the fix work.<br/>
<br/>
I'll see if I can knock off those two issues this weekend and think about how I want to approach the UI; I'm thinking somewhere along the lines of "clean, modern, easy to use, a <a href="https://en.wikipedia.org/wiki/White-label_product">white label</a>-ish prototype-ly kinda feel".<br/>
<br/>
Can I get to v0.3.0 by next Saturday? I'm optimistic I can, but that optimism is derived entirely from the lack complexity of the work I make for myself in the intervening time. We'll see.<br/>
<br/>
Right now, though, I'll settle for a few Zzz's<br/>
<br/>
</details>
<br/>
<details>
<summary>Wednesday, November 21, 2018</summary>
<br/>
I've managed to take two steps back from yesterday's step forward and once again find myself dealing with non-restarting contracts. Some of the same symptoms have re-appeared but this time as a result of changes to other sections of the code.<br/>
<br/>
It's quite surprising to see that yesterday's changes worked as well as they did considering that I'd failed to update parts of the <a href="https://github.com/monicanagent/cypherpoker.js/blob/master/src/web/scripts/CypherPokerContract.js">CypherPokerContract</a> class to insolate the contract data from the game.<br/>
<br/>
Oh well, them's the breaks--it is a pretty complex piece of code after all. I can at least console myself with the fact that the contract interactions in CypherPoker.AS were also the trickiest parts to deal with and took a while to iron out. In addition, I can retrace my latest fixes since the issues seem to be appearing in the same locations (albeit not in exactly the same way).<br/>
<br/>
I've updated the <a href="https://github.com/monicanagent/cypherpoker.js/milestone/1">v0.2.3 milestone</a> to end this coming Friday and hopefully I can hit that date. It'll be about a week late but it should still leave sufficient time for the v0.3.0 updates. Fingers crossed.<br/>
</details>
<br/>
<details>
<br/>
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
