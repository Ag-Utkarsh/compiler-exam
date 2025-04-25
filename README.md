def left_factor(grammar):
    new_grammar = {}
    for non_terminal, productions in grammar.items():
        prefix_map = {}
        
        # Group productions by common prefixes
        for prod in productions:
            prefix = prod[0] if prod else ""
            if prefix not in prefix_map:
                prefix_map[prefix] = []
            prefix_map[prefix].append(prod)

        # If all productions have unique prefixes, no left factoring needed
        if len(prefix_map) == len(productions):
            new_grammar[non_terminal] = productions
            continue

        # Create new rules after left factoring
        new_productions = []
        new_non_terminal = non_terminal + "'"
        factored_productions = []

        for prefix, group in prefix_map.items():
            if len(group) > 1:
                # Extract common prefix
                remaining_parts = [prod[1:] if len(prod) > 1 else "" for prod in group]
                new_productions.append(prefix + new_non_terminal)
                factored_productions.extend(remaining_parts)
            else:
                new_productions.append(group[0])

        new_grammar[non_terminal] = new_productions
        new_grammar[new_non_terminal] = factored_productions

    return new_grammar


def input_grammar():
    grammar = {}
    print("Enter grammar rules (e.g., A -> ab | ac). Type 'done' when finished:")
    
    while True:
        rule = input().strip()
        if rule.lower() == "done":
            break
        lhs, rhs = rule.split("->")
        lhs = lhs.strip()
        rhs_productions = [prod.strip() for prod in rhs.split("|")]
        grammar[lhs] = rhs_productions
    
    return grammar


# Get grammar input
grammar = input_grammar()
print("\nOriginal Grammar:")
for nt, prods in grammar.items():
    print(f"{nt} -> {' | '.join(prods)}")

# Perform left factoring
factored_grammar = left_factor(grammar)

# Display the result
print("\nLeft Factored Grammar:")
for nt, prods in factored_grammar.items():
    print(f"{nt} -> {' | '.join(prods)}")
