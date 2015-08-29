# Themis Attack Protocol
**FYI:** This document is also available in [Russian language](README_RU.md).

This repo contains the instructions for submitting captured flags to the [Themis Finals](https://github.com/aspyatkin/themis-finals) - CTF contest checking system.

This system will be used on [VolgaCTF 2015 Finals](http://volgactf.ru) contest on September 10, 2015.

It is common to use Telnet for submitting flags on the CTF contests. However, [Themis Finals](https://github.com/aspyatkin/themis-finals) developers prefer the other way.

## Prerequisites
1. You should know a contest checking system's IP address.
2. You should have a command-line utility capable of sending HTTP requests.

## Submitting flags
Assuming contest checking system IP address is `10.0.0.2` and you have [curl](http://curl.haxx.se) installed:  
1. Capture flags. Remember, a valid flag should match the following regexp: `/^[\da-f]{32}=$/`.  
2. Construct JSON array of flags. For instance,
`["b5c8b7c23cec74a903f764ec202d7c5c=","2bc1da92090e8b13d2950fc517752eea="]`  
3. Perform an HTTP request:  
`$ curl -X POST -v -H "Content-Type: application/json" -d "[\"b5c8b7c23cec74a903f764ec202d7c5c=\",\"2bc1da92090e8b13d2950fc517752eea=\"]" http://10.0.0.2/api/submit`  
4. Examine response. You will get a single number if a general error occured (e.g. data formatting error, or contest pause). Otherwise you will get an array of numbers, one for attack result of each flag. See possible result codes below.

## Attack result codes
| Code | Description |
|------|-------------|
|0|Submitted flag has been accepted|
|1|Generic error|
|2|The attacker does not appear to be a team|
|3|Contest has not been started yet|
|4|Contest has been paused|
|5|Contest has been completed|
|6|Submitted data has invalid format|
|7|Attack attempts limit exceeded|
|8|Submitted flag has expired|
|9|Submitted flag belongs to the attacking team and therefore won't be accepted|
|10|Submitted flag has been accepted already|
|11|Submitted flag has not been found|
|12|The attacking team service is not up and therefore flags from the same services of other teams won't be accepted|

## Notes
1. Request payload is limited to 1024 bytes. You can safely pass up to 25 flags at once.
2. Before submitting flags captured from your competitor's service `N`, please assure that **your** service `N` is up and running.
3. There are some limitations to flag submissions. You can make no more than `X` attack attempts (one flag) in the last `Y` seconds. Both successful and unsuccessful attempts are counted. For instance, if `X` is 100 and `Y` is 60, you can make 100 attack attempts in a minute (if you send 25 flags at each request, you can make 4 requests in a minute). Contest organizing committee should clarify the values of `X` and `Y` for contestants.

## License
MIT @ [Alexander Pyatkin](https://github.com/aspyatkin)
