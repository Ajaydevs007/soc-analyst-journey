# Event Correlation

## Definition

Event correlation is the process of connecting multiple events or logs together to identify meaningful patterns, suspicious activity, or cyber attacks.

### Why Event Correlation Is Important

A single event may appear harmless.

Multiple related events may indicate an attack.

---

## Example

### Individual Events

```text
Failed Login
Failed Login
Failed Login
Failed Login
Failed Login
```

### Correlated Result

```text
Possible Brute Force Attack
```

---

## Types of Event Correlation

### 1. Rule-Based Correlation

Uses predefined conditions or rules.

#### Example

```text
IF Failed Logins > 5
THEN Generate Alert
```

#### Use Case

Detect brute-force login attempts.

---

### 2. Time-Based Correlation

Analyzes events occurring within a specific time window.

#### Example

```text
100 Requests Within 10 Seconds
```

#### Possible Threats

- Denial of Service (DoS)
- Brute Force Attack

---

### 3. Behavior-Based Correlation

Detects abnormal user or system behavior.

#### Example

```text
User normally logs in from India

Suddenly logs in from Russia

Generate Alert
```

#### Use Case

Detect account compromise or stolen credentials.

---

## Real SOC Example

### Events

```text
Failed Login
Failed Login
Failed Login
Successful Login
PowerShell Execution
Suspicious Network Connection
```

### Correlation Result

```text
Possible Account Compromise
```

---

## Key Takeaway

Event correlation helps SOC analysts connect multiple events and identify attacks that may not be visible from a single log entry.
