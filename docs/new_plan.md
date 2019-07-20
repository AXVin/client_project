# Plans
This is the Plan Document for the Tournament and Product Integration

# Database Design
> Database Used: PostgreSQL

Table Name: coupons

| Key | Type | Description |
| --- | ---- | ----------- |
| coupon | `VARCHAR(16) PRIMARY KEY` | An 8 bit token_hex used to redeem the roles |

## Creation of Token
```python
>>> import secrets
>>> secrets.token_hex(8)
```


# Discord Server Design
The discord server plan where the tournament will be held

## Roles

### Staff Structure

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


Above are the required/mandatory channel(s).
Rest are Optional and can be created using Channel Overrides for the Roles


# Bot Interface
Prefix = `!`

## Commands
* `redeem <coupon>`: Grants access to the Community role


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



###