# INSTALLATION

!!! note
    Do these steps in the order specified

## Pre-requisites

* Python v3.7+
* PostgreSQL v10+

## Installing dependencies

* `pip install --editable .`

## Configuring your database
After installing PostgreSQL, Open your `psql` tool and type in

```sql
CREATE ROLE coupons WITH LOGIN PASSWORD 'password';
CREATE DATBASE coupons OWNER coupons;
```

This will create the role `coupons` with the password `password` and a database named `coupons`

## Creating your Bot

* Go on the [Discord's Developer](https://discordapp.com/developers) site
* Create an Application. You can name it anything you want
* Click on `Bots` tab on the left drawer
* Now on the `ADD-A-BOT` page, click on the `Add Bot` button and confirm on the next dialogue
* Copy the Bot Token from the next page

Make sure you don't copy the client secret instead of the Bot Token

## Writing the configuration file
This is the heart and soul of the bot!

Edit the file named `config.py` situated in the main directory as follows

```py
token = '' # The Bot Token
postgres = 'postgres://coupons:password@localhost/coupons'
verified_role = 597308576456769556 # The ID of the role which is assigned on claiming a coupon successfully

welcome_channel = 597308294687752230 # The ID of the channel where the welcome message is to be sent
welcome_message = '''Welcome {user.mention}! Thanks for joining the {server}! We are now at {member_count:,d} members.
Please make sure to redeem your coupon which is on the back of the product's label with the `!redeem <coupon>` command!
For example: `!redeem GHD387HDFK89`
'''
# The welcome message sent whenever a user joins!
# Variable:
#       {user} = The user who joined
#           You can use .mention on it to ping the user
#       {server} = The server which the user joined
#       {member_count} = The current member count!

error_logs   = 597377084960014355    # The ID of the channel where the errors are sent(if any)
commands_debug  = 597377182720983070 # The ID of the channel where all the commands are logged!
``` 

## Running the Bot

!!! warning
    Must run the bot once before creating any coupons/batches. This ensures that the database is completly set up

* Start the bot with `coupons run` command

## Creating Coupons
You can create coupons with the `coupons create` command.

!!! note
    Each command creates a new batch

--file-format defaults to csv
--output-path defaults to `./coupons/` directory

```
$ coupons create --help
Usage: coupons create [options]

  Creates Coupons

Options:
  -a, --amount INTEGER         number of coupons to create  [required]
  -f, --file-format [csv|txt]  format in which the coupon is outputted
  -o, --output-path PATH
  --help                       Show this message and exit.
```

For example:-

* Simple: `coupons create --amount 100` (Can omit the --amount part to start a prompt)
* Txt output: `coupons create -a 100 -f txt`
* File on desktop: `coupons create -a 100 -o C:\Users\my_user\Desktop\`

File Path works on all operating systems

All flags are optional and the order in which you put them doesn't matter

## Deleting the entire database of coupons and batches

!!! danger
    This action is irreversible and will delete EVERYTHING from the database

Run `coupons clear_database` in your terminal to clear the database