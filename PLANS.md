# Plans
This is the Plan Document for the Tournament and Product Integration

# Database Design
> SQL Database design
Table Name: product_integration (Optional)

| Key | Type | Description |
| --- | ---- | ----------- |
| purchase_id | `PRIMARY KEY` | Purchase ID |
| product_id | `INTEGER` | Foreign Key to the purchased product (Optional) |
| coupon | `VARCHAR` | A Randomly Generated key used to redeem the roles |
| used | `BOOLEAN` | A bool to indicate if the coupon has already been used or not. Optional, could be stored on bot's database, included here for security purposes |
| role_id | `BIGINT` | The role which will be assigned on redeeming the coupon |

## Format for coupon
Coupon can be a hex token(4-8 bytes to keep it User-Friendly)


# API Design
A RESTful API for verification of the user inputted coupon to prevent any security issues from direct access to the database
Requires a valid `client_id`(randomly generated, token hex[16-32 bytes]) for authorization

## Endpoints:
If using a subdomain for only this API, the prefix `/coupon` could be omitted

### POST `/coupon/verify`
For verification of the user-inputted coupon

**Headers:**
> `Authorization: client_id`

**Body:**
```json
{
    "coupon": <COUPON>
}
```

**Returns:**
```json
{
    "role_id": <ROLE_ID>,
    "purchase_id": <PURCHASE_ID>   // Optional
}
```

**Raises:**
* `403`: Invalid client_id: Authorization Failed
* `404`: Invalid/Used coupon: The coupon does not exist or has already been used(If used is True) 

**Backend Action:**
1. Check client_id and authorize (`403` if invalid)
2. Query database (`404` if no record found)
3. Check `used` field in row (`404` if used is True)
4. Return the json response


### POST `/coupon/used`
For turning the `used` field in database to True after successfully assigning roles

**Headers:**
> `Authorization: client_id`

**Body:**
```json
{
    "coupon": <COUPON>
}
```

**Returns:**
* `200`: Successful

**Raises:**
* `403`: Invalid client_id: Authorization Failed
* `404`: Invalid/Used coupon: The coupon does not exist or has already been used(If used is True) 

**Backend Action:**
1. Check client_id and authorize (`403` if invalid)
2. Query database (`404` if no record found)
3. Check `used` field in row (`404` if used is True)
4. Sets `used` field in the database to True


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
* `help`: Shows the bot's commands
* `redeem <coupon>`: Grants access to the Community role


### `!redeem <coupon>` command
**Arguments:**
* coupon: The coupon recieved on the purchase of the product

**Returns:**
* Assigns role to the user on success

**Backend Actions:**
1. Takes user input
2. Hits the `/coupon/verify` endpoint with client_id and coupon
3. If successful:
    * Assign Role to the user
    * Hits the `/coupon/used` endpoint with client_id and coupon

