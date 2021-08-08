- This guid is intended for ethical activities so please use it for ethical testing only.
- I wrote this methodology as a guide for testing API collections summerizing many resources in addition to my own personal experience. I wrote it as simple and as direct as possible to be available for every one to use it, dispite the level of experience, and I hope it would be of great help for anyone who needs a starting point to test APIs -wether you are a penetration tester or a bug bounty hunter.
- Any contribution is very appriciated, as well as any advice or comment.

==================================================================================

# Methodology
Before you start please familiarize yourself with the [OWASP Top 10 API Vulnerabilities](https://owasp.org/www-project-api-security/), and other types of vulnerabilities that you want to look for in APIs (like CORS and SSRF).

## STEP-1- Fuzzing and enumeration (Black box / Bug bounty hunting)
When it comes to APIs, this is almost the most important step. You can’t hack what you don't know of, so first use your favorite fuzzing tool to fuzz the following in order:
1. Endpoints (basically URIs/subdirectories), also:
  - Look for different versions of the APIs.
  - Look for different types of files.
2. Parameters.

**TIP1:** for endpoints enumeration use the `HEAD` method, it’s just faster and gives you the `status 200` you’re looking for. (But first make sure it works! Some APIs respond with “200” to any request with a method other than the intended one)
**Note1:** you need to build one wordlist for endpoints and a whole other one for parameter names (or download them from the web). Go for words that are as suitable as possible to the API that you are testing –example: for banks your wordlist should include “accounts”, ”clients”, ”users”, ”balance” and so on, you got the idea.

## STEP-2- Know what you are dealing with
Now that you have all the endpoints that you can request, and the parameters you need to successfully call them, well, go ahead and call them! In this step you need to try to understand what you are dealing with. So, get a normal healthy response from every endpoint. To ease your next phase I suggest you take a look at the responses and look for any “Excessive Data Exposure” in other words, what information in the response that could be sensitive and/or should not be there? Did you find something? Well! You just found one of the Top 10 OWASP API Vulnerabilities. Along with that, I also suggest you mark the endpoints that you find interesting so that you get back to them later to look for other vulnerabilities.

## STEP-3- Attack and Manipulation
Now that you know what you’re dealing with, and maybe already found some leaks, it’s time for the dirty stuff.
### Where do you want to look?
- Methods: replace `post` ⬄ `delete` // `post` ⬄ `get` // `post` ⬄ `put` // `delete` ⬄ `put`...
- Headers.
- Parameter (names & values)
  1. Change.
  2. Bypass/delete.
  3. Duplicate.
### What do you want to try?
1. Values/payloads with different and not expected length, range, format, type…
2. Overflow.
3. Rate limiting (if allowed).
4. XXE: changing the content type to `application/xml`, add XML request body and see how request respond. If its throwing XML based error you can try for XML Entity Attacks.
5. File uploads.
6. Injection.
7. Token reuse (save a file of over 200 tokens and try them).
8. Change the referrer.

## Additional Notes and Tips:
1. Do the intelligence gathering phase: any info could help you (even if it’s out of scope.. you are just gathering info not breaking any rules).
2. Ideally: understand roles, resources, and the functionality of the application, and the responses of each endpoint. Also, perform tests on each of the endpoints too. (sometimes it is not possible to test all endpoints, this is where the endpoints you’ve marked in step2 come to use)
3. Headers are not just for manipulation. Certain values for the headers, or missing headers, can lead to security vulnerabilities, so don’t forget to look for those (see the [Secure Headers Project](https://owasp.org/www-project-secure-headers/) by OWASP).
4. Management endpoints: Sometimes management endpoints are needed to be accessible via the Internet, make sure to check for them and test if they have the strong authentication mechanism, e.g. multi-factor. They can (and should) be exposed via different ports or hosts preferably on a different NIC and restricted subnet. Access to these endpoints should be restriceted by firewall rules or use of access control lists.

==================================================================================

## Tools:
- [Burp suite](https://portswigger.net/burp/communitydownload) with Extensions like “Authorize”, “JWT Attacker”, “Auto Repeater”, “Turbo intruder”. ⇒ ⇒ _The well-known web proxy_
- [Postman](https://www.postman.com/) (proxy it throw burp) ⇒ ⇒ API _development and testing platform, with GUI_.
- [MindAPI](https://dsopas.github.io/MindAPI/play/) ⇒ ⇒ _API testing framework (similar to OSINT)_
- [Header scanner](https://securityheaders.com/) ⇒ ⇒ _Tool to scan for security headers_
- [Astra](https://github.com/flipkart-incubator/Astra) ⇒ ⇒ _Automated tool for web application and APIs scanning, with GUI_
- [FuzzAPI](https://github.com/Fuzzapi/fuzzapi) ⇒ ⇒ _Fuuzing tool for APIs, with GUI_
- [JWT](https://jwt.io/) ⇒ ⇒ _Website for decoding and dehashing JWTs (JSON Web Tokens)_
- [sqlmap](https://sqlmap.org/) ⇒ ⇒ _The well-known sql injection scanner_
- [Vooki](https://www.vegabird.com/vooki/) ⇒ ⇒ _Automated tool for web application and APIs scanning, with GUI_
- API word lists ⇒ ⇒ _Creat or download wordlists from the web_
- [Swagger](https://swagger.io/) ⇒ ⇒ _API development platform with many tools and also valuable blog posts that can help you_
- [Parameth](https://github.com/maK-/parameth) ⇒ ⇒ _Parameter fuzzing tool_
- [Js-scan](https://github.com/zseano/JS-Scan) ⇒ ⇒ _Tool to extract APIs from JS files_
- [Trufflehog](https://github.com/trufflesecurity/truffleHog) ⇒ ⇒ _An effective tool that search Git repos and commit histories for secrets_ 

## Advance topics: (for contributors and later editions on API testing)
- Ssl
- JWT
- API keys
- Mobile applications
- Git repos

## References:
- https://medium.com/@Johne_Jacob/api-penetration-testing-things-to-be-noted-14ec0a170222
- https://owasp.org/www-project-api-security/
- https://github.com/OWASP/API-Security
- https://cheatsheetseries.owasp.org/cheatsheets/REST_Security_Cheat_Sheet.html
- https://owasp.org/www-project-secure-headers/
- https://www.youtube.com/watch?v=5UTHUZ3NGfw 
- https://www.youtube.com/watch?v=ijalD2NkRFg
- https://www.youtube.com/watch?v=zW8QF3x3oSU
