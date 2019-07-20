# Discord Server Design
The discord server plan where the tournament will be held

## Roles

### Staff Structure

??? note "Note"
    These are optional

* `Tournament Admins` - Have full access/control of server and can create bot preferences
* `Tournament Moderators` - Limited Access to the server and will maintain a link between Admins and users

### General

* `Community`/`Verified` - Role assigned when a coupon is successfully redeemed
* `Tournament Participant` - Role assigned once the user verifies that they want to join the tournament


## Channels

`welcome` - Greets the new user and asks them to run the `!redeem <coupon>` command
> Access Level - Public (everyone)

`room-pass-and-id` - Channel where the users will be informed about the 
> Access Level - Tournament Participant(Role)

### Bot Channels

`error-logs` - If any unhandled errors occur, they are shown here
> Access Level - Bot, Admins

`commands-debug` - All commands invoked in the server are shown here!
> Access Level - Bot, Admins


Above are the required/mandatory channel(s).
Rest are Optional and can be created using Channel Overrides for the Roles


# Bot Interface
Prefix = `!`


## Main Commands
* `redeem <coupon>`: Grants access to the Community role


??? example "Example"
    `!redeem GHDIJH34HA8F`


### `!redeem <coupon>` command
#### Arguments:
* coupon: The coupon recieved on the purchase of the product

#### Returns:
* Assigns role to the user on success

#### Backend Actions:
1. Takes user input
2. Run `SELECT * FROM coupons WHERE coupon=$input;`
3. If returned:
    * Assign `Community/Verified` Role to the user
    * Delete the coupon from the database to preserve space
