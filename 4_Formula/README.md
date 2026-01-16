# 📚 4_Formula: Guides & Best Practices

- **Premise:** Don't reinvent the wheel.
- **Content:** Essential guides, formulas, and code snippets.
- **Conclusion:** Solves challenges efficiently and ensures high quality.

## 🧪 The Formulas (Methodologies)

### The 4-Phase Red Team Methodology

#### Phase 1: Planning & Threat Modeling
```python
# Define adversary profile
adversary = {
    'motivation': 'Bypass content moderation to post toxic content',
    'capability': 'Medium (can modify text, no model access)',
    'resources': 'Public tools, basic scripting',
    'constraints': 'Must maintain readability to humans'
}

# Define attack goal
attack_goal = {
    'objective': 'Evade toxic content classifier',
    'success_criteria': 'Toxic comment classified as non-toxic',
    'detection_threshold': 'Model confidence < 0.5 for toxic class'
}

# Assess impact
impact_assessment = {
    'severity': 'High',
    'affected_users': 'All platform users',
    'business_impact': 'Reputational damage, user churn,
    'regulatory_risk': 'Potential violation of content policies'
}
```

#### Phase 2: Reconnaissance
```python
# Understand model behavior
def probe_model(model, test_inputs):
    """Probe model to understand decision boundaries."""
    results = []
    for text in test_inputs:
        prediction = model.predict([text])[0]
        confidence = max(prediction)
        results.append({
            'input': text,
            'prediction': 'toxic' if prediction[1] > 0.5 else 'non-toxic',
            'confidence': confidence
        })
    return results

# Map attack surface
attack_surface = {
    'input_interface': 'Text API endpoint',
    'input_validation': 'Length check only',
    'output': 'Binary classification + confidence',
    'rate_limiting': 'None observed',
    'monitoring': 'Unknown'
}
```

#### Phase 3: Exploitation - Evasion Attacks
```python
# Character Substitution Attack
def character_substitution(text):
    """Replace characters with similar-looking alternatives."""
    substitutions = {
        'a': '@',
        'e': '3',
        'i': '1',
        'o': '0',
        's': '$',
        't': '7'
    }
    evaded = text.lower()
    for char, replacement in substitutions.items():
        evaded = evaded.replace(char, replacement)
    return evaded

# Case Variation Attack
def case_variation(text):
    """Alternate character case to evade pattern matching."""
    return ''.join([c.upper() if i % 2 == 0 else c.lower() 
                   for i, c in enumerate(text)])

# Homoglyph Attack
def homoglyph_substitution(text):
    """Replace characters with visually similar Unicode characters."""
    homoglyphs = {
        'a': 'а',  # Cyrillic 'a'
        'e': 'е',  # Cyrillic 'e'
        'o': 'о',  # Cyrillic 'o'
        'p': 'р',  # Cyrillic 'p'
    }
    evaded = text
    for char, replacement in homoglyphs.items():
        evaded = evaded.replace(char, replacement)
    return evaded

# Space Insertion Attack
def space_insertion(text):
    """Insert spaces between characters to break word patterns."""
    return ' '.join(list(text))

# Combined Attack
def combined_evasion(text):
    """Apply multiple evasion techniques."""
    # Apply character substitution
    evaded = character_substitution(text)
    # Add random spaces
    evaded = ' '.join([evaded[i:i+2] for i in range(0, len(evaded), 2)])
    return evaded
```

#### Phase 4: Reporting - Vulnerability Assessment
```python
# Vulnerability severity calculation
def calculate_severity(likelihood, impact):
    """
    Calculate vulnerability severity using risk matrix.
    
    Likelihood: 1-5 (1=rare, 5=certain)
    Impact: 1-5 (1=minimal, 5=catastrophic)
    
    Returns: Severity level (Critical/High/Medium/Low)
    """
    risk_score = likelihood * impact
    
    if risk_score >= 20:
        return 'Critical'
    elif risk_score >= 12:
        return 'High'
    elif risk_score >= 6:
        return 'Medium'
    else:
        return 'Low'

# Generate vulnerability report
def generate_vulnerability_report(attack_results):
    """Create structured vulnerability report."""
    report = {
        'vulnerability_id': 'ML-EVA-001',
        'title': 'Content Moderation Evasion via Character Substitution',
        'severity': calculate_severity(likelihood=4, impact=5),
        'description': 'Model fails to detect toxic content when characters are substituted with similar-looking alternatives',
        'attack_success_rate': attack_results['success_rate'],
        'affected_component': 'Text classifier',
        'reproduction_steps': attack_results['steps'],
        'mitigation': 'Implement text normalization, train on obfuscated examples, use ensemble models'
    }
    return report
```

### Attack Success Metrics

#### Evasion Success Rate
```python
evasion_success_rate = (evaded_correctly_classified / total_toxic_samples) * 100
```

#### Attack Effectiveness Score
```python
# Combines success rate with stealthiness (human readability)
effectiveness = (success_rate * 0.7) + (readability_score * 0.3)
```

#### Severity Score
```python
severity_score = (attack_success_rate * likelihood * impact) / 100
```

## 📐 Threat Modeling Framework

### STRIDE for ML Systems
-   **S**poofing: Impersonate legitimate users/data
-   **T**ampering: Modify training data or model parameters
-   **R**epudiation: Hide attack traces
-   **I**nformation Disclosure: Leak training data or model
-   **D**enial of Service: Overwhelm system resources
-   **E**levation of Privilege: Gain unauthorized access

### Attack Taxonomy
1.  **Evasion**: Fool model at inference time
2.  **Poisoning**: Corrupt model during training
3.  **Extraction**: Steal model or training data
4.  **Inference**: Learn sensitive info from model outputs
