## So I have this bot, Now What?
### What does this bot need to do?

* register new users
* collect game info
* start a new game
* collect kills
* confirm kills
* determine winner
* react to player commands to view/update info

### What tools does this bot need to do its job

* send slack messages
* recieve slack messages
* authenticate slack user -> uid using http api
* http api to recieve webhooks
* persistent storage via sqldb

### So how is this bot going to accomplish its goals using those tools

* register user
	* recieve `@bot register` via slack
	* `POST` to auth with slack, to recieve token
	* send slack private message to user confirming register
* collect game info
	* send info notice w/ command help
	* recieve `!weapon set` or `!location set`
	* update database
	* echo updated information via slack
* start a new game
	* monitor number of free players
	* generate starting state
	* update database
	* notify players of inital state via slack
* collect kills
	* recieve `!report kill` via slack
		* mark hit as pending
		* init confirm
	* recieve `!report override` via slack
		* if correct
			* mark hit as confirmed
		* if wrong
			* 3 tries, then reporter dies
* confirm kill
	* send confirmation msg to target via slack
	* recieve `!confirm` or `!deny`
	* if confirm
		* mark hit as complete
		* assign new hit
	* if deny
		* notify reporter of deny prompt for override
* determine the winner
	* monitor confirmed deaths
	* if one man standing
		* user wins
		* notify everyone via slack
* react to player commands
	* monitor slack
	* respond via slack


### So what does the new game flow need to look like?

* registration
	* remove location, as no longer needed
	* same except for the addition of peer confirmed weapons

* starting a game
	* there will be only one continuous game (+ one for piscine only?)
	* new users enter field immediatly upon registration completion

* assignments
	* assignments will be issued at 11:42 PM Sun - Thur and last 48 hours.
	* hitmen can report missing their target missing in the 12 hours following the closure of an assignment.
	* reporting now includes a photo of hitman, target + weapon
	* reports will be peer confirmed
	* cool off for failed

* scoring
	* Per player Kill/death counter
	* points for successful assignment
	* global leaderboard

	