# Investigation: Coldplay Song Used as Brute Force Password

## Investigation ID

INV-015

---

# Objective

Identify the password used during the brute-force attack that matches James Brodsky's favorite Coldplay song.

---

# Question

> One of the passwords in the brute force attack is James Brodsky's favorite Coldplay song. Hint: we are looking for a six character word on this one. Which is it?

---

# Background

Previous investigations identified the brute-force attack against the Joomla administrator interface.

Known attacker:

```text
23.22.63.114
```

Target:

```text
192.168.250.70
```

Authentication endpoint:

```text
/joomla/administrator/index.php
```

The previous investigation also established that the attempted passwords are contained in the HTTP `form_data` field.

---

# Data Source

- Index: `botsv1`
- Sourcetype: `stream:http`

---

# Investigation Steps

## Step 1 - Identify Brute Force Authentication Requests

```spl
index=botsv1
sourcetype="stream:http"
src_ip="23.22.63.114"
dest_ip="192.168.250.70"
http_method=POST
username
passwd
```

The results contain repeated authentication attempts against the Joomla administrator interface.

---

## Step 2 - Extract the Password

The password is contained inside the `form_data` field.

```spl
index=botsv1
sourcetype="stream:http"
src_ip="23.22.63.114"
dest_ip="192.168.250.70"
http_method=POST
username
passwd
| rex field=form_data "passwd=(?<passwd>\w+)"
| table _time passwd
```

---

## Step 3 - Filter for Six-Character Passwords

The question specifies that the password is exactly six characters long.

```spl
index=botsv1
sourcetype="stream:http"
src_ip="23.22.63.114"
dest_ip="192.168.250.70"
http_method=POST
username
passwd
| rex field=form_data "passwd=(?<passwd>\w+)"
| where len(passwd)=6
| table _time passwd
| sort passwd
```

This produces the six-character password candidates.

---

# Step 4 - Compare Against Coldplay Songs

The remaining candidates were compared against Coldplay song titles.

Relevant six-character candidates include:

```text
Clocks
Oceans
Sparks
Shiver
Yellow
```

The password appearing in the brute-force traffic that matches a Coldplay song is:

```text
yellow
```

Independent BOTS v1 walkthroughs use the same approach: extract `passwd`, restrict it to six characters, then identify `yellow` as the matching Coldplay song. :contentReference[oaicite:1]{index=1}

---

# Findings

The six-character password matching a Coldplay song was:

```text
yellow
```

---

# Investigation Flow

```text
Brute Force HTTP Traffic
        |
        v
form_data
        |
        v
Extract passwd
        |
        v
Filter len(passwd) = 6
        |
        v
Compare with Coldplay songs
        |
        v
YELLOW
```

---

# Evidence

| Field | Value |
|---|---|
| Attacker IP | `23.22.63.114` |
| Target IP | `192.168.250.70` |
| Protocol | HTTP |
| Method | POST |
| Authentication | Joomla Administrator |
| Password Length | 6 |
| Matching Song | Yellow |
| Password | `yellow` |

---

# Conclusion

The password used during the brute-force attack that matches James Brodsky's favorite six-character Coldplay song was:

```text
yellow
```