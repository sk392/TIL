###  UniqueEmailAddresses


요번엔 [UniqueEmailAddresses.kt](https://leetcode.com/problems/unique-email-addresses/)요거를 풀어봐씁니다.

### Problem
Every email consists of a local name and a domain name, separated by the @ sign.

For example, in alice@leetcode.com, alice is the local name, and leetcode.com is the domain name.

Besides lowercase letters, these emails may contain '.'s or '+'s.

If you add periods ('.') between some characters in the local name part of an email address, mail sent there will be forwarded to the same address without dots in the local name.  For example, "alice.z@leetcode.com" and "alicez@leetcode.com" forward to the same email address.  (Note that this rule does not apply for domain names.)

If you add a plus ('+') in the local name, everything after the first plus sign will be ignored. This allows certain emails to be filtered, for example m.y+name@email.com will be forwarded to my@email.com.  (Again, this rule does not apply for domain names.)

It is possible to use both of these rules at the same time.

Given a list of emails, we send one email to each address in the list.  How many different addresses actually receive mails? 

 

```
Example 1:

Input: ["test.email+alex@leetcode.com","test.e.mail+bob.cathy@leetcode.com","testemail+david@lee.tcode.com"]
Output: 2
Explanation: "testemail@leetcode.com" and "testemail@lee.tcode.com" actually receive mails
``` 

```
Note:

1 <= emails[i].length <= 100
1 <= emails.length <= 100
Each emails[i] contains exactly one '@' character.
All local and domain names are non-empty.
Local names do not start with a '+' character.

```

### Solution

```
import java.util.HashSet

class Solution {
    fun numUniqueEmails(emails: Array<String>): Int {
        val uniqueEmails = HashSet<String>()
        for (email in emails) {
            uniqueEmails.add(cleanEmail(email))
        }
        return uniqueEmails.size
    }

    private fun cleanEmail(email: String): String {
        var clean = email.substring(0, email.indexOf("@")).replace(".", "")
        if (clean.contains("+")) clean = clean.substring(0, clean.indexOf("+"))
        return clean + email.substring(email.indexOf("@"))
    }
}
```

### Explanation

주말에 간단하게 몰아서 해결하기!!ㅋㅋㅋㅋ 이메일 정규화하는 작업의 메서드 하나빼고 해시셋에 넣어서 중복제거해서 사이즈로 쟀다 !