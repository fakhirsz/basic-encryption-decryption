[caesar_cipher.py](https://github.com/user-attachments/files/29402984/caesar_cipher.py)
"""
Basic Encryption & Decryption — Caesar Cipher
DecodeLabs Industrial Training Kit - Project 2

Goal: Encrypt and decrypt text using the Caesar Cipher.

Key concepts demonstrated:
  - IPO Model: Input (plaintext + key) → Process (shift) → Output (ciphertext)
  - ASCII logic: ord() converts a character to its integer value;
                 chr() converts an integer back to a character.
  - Modular arithmetic (% 26) wraps the alphabet so Z + 3 = C, not a
    character outside the alphabet.
  - Symmetric encryption: the same shift key that locks the data unlocks it.
  - Edge case handling: spaces, digits, and punctuation pass through unchanged
    so the cipher only touches letters.
  - Bonus: a brute-force demo that reveals all 25 possible decryptions —
    illustrating exactly why the Caesar Cipher is educational but NOT secure.
"""


# ── Core cipher functions ────────────────────────────────────────────────────

def caesar_encrypt(plaintext: str, shift: int) -> str:
    """
    Encrypt plaintext using a Caesar Cipher with the given shift key.

    Formula (as shown in the brief):
        E_n(x) = (x + n) % 26
    where x is the letter's 0-based index (A=0 … Z=25) and n is the shift.

    Uppercase and lowercase letters are shifted independently so that case
    is preserved.  All other characters (spaces, digits, punctuation) are
    left unchanged — this is the "handle edge cases" deliverable.
    """
    ciphertext = []

    for char in plaintext:
        if char.isupper():
            # Shift within A-Z range
            shifted = (ord(char) - ord('A') + shift) % 26
            ciphertext.append(chr(shifted + ord('A')))

        elif char.islower():
            # Shift within a-z range
            shifted = (ord(char) - ord('a') + shift) % 26
            ciphertext.append(chr(shifted + ord('a')))

        else:
            # Space, digit, or symbol — pass through untouched
            ciphertext.append(char)

    return ''.join(ciphertext)


def caesar_decrypt(ciphertext: str, shift: int) -> str:
    """
    Decrypt ciphertext by reversing the shift.

    Formula (from the brief):
        D_n(x) = (x - n) % 26

    Decryption is simply encryption with a negative shift, so we reuse
    the same function — this demonstrates symmetric encryption:
    'the same key that locks the data unlocks it.'
    """
    return caesar_encrypt(ciphertext, -shift)


def brute_force(ciphertext: str) -> None:
    """
    Try all 25 possible shift values and print each result.

    This is the 'vulnerability demo' from the brief's slides:
    because the key space is only 25 keys, an attacker can instantly
    brute-force every possibility and spot the readable output by eye.
    """
    print("\n  [Brute-Force Demo — all 25 possible decryptions]")
    print("  " + "-" * 50)
    for key in range(1, 26):
        attempt = caesar_decrypt(ciphertext, key)
        print(f"  Shift {key:>2}: {attempt}")
    print("  " + "-" * 50)
    print("  Notice: one of those lines is the original plaintext.")
    print("  This is why Caesar Cipher is educational, not secure.\n")


# ── Menu / main loop ─────────────────────────────────────────────────────────

def get_shift_key() -> int:
    """Prompt the user for a shift key and validate the input."""
    while True:
        try:
            shift = int(input("  Enter shift key (1-25): "))
            if 1 <= shift <= 25:
                return shift
            print("  Please enter a number between 1 and 25.")
        except ValueError:
            print("  Invalid input — please enter a whole number.")


def main():
    print("=" * 55)
    print("  DecodeLabs — Basic Encryption & Decryption")
    print("  Caesar Cipher | Project 2")
    print("=" * 55)

    while True:
        print("\nWhat would you like to do?")
        print("  1. Encrypt a message")
        print("  2. Decrypt a message")
        print("  3. Encrypt then decrypt (full demo)")
        print("  4. Brute-force a ciphertext (vulnerability demo)")
        print("  5. Quit")

        choice = input("\nEnter choice (1-5): ").strip()

        if choice == '1':
            text = input("\n  Enter plaintext : ")
            shift = get_shift_key()
            encrypted = caesar_encrypt(text, shift)
            print(f"\n  Original  : {text}")
            print(f"  Encrypted : {encrypted}")
            print(f"  Shift Key : {shift}")

        elif choice == '2':
            text = input("\n  Enter ciphertext : ")
            shift = get_shift_key()
            decrypted = caesar_decrypt(text, shift)
            print(f"\n  Ciphertext : {text}")
            print(f"  Decrypted  : {decrypted}")
            print(f"  Shift Key  : {shift}")

        elif choice == '3':
            text = input("\n  Enter plaintext : ")
            shift = get_shift_key()
            encrypted = caesar_encrypt(text, shift)
            decrypted = caesar_decrypt(encrypted, shift)
            print(f"\n  Original  : {text}")
            print(f"  Encrypted : {encrypted}")
            print(f"  Decrypted : {decrypted}")
            print(f"  Shift Key : {shift}")
            match = "✓ Match" if text == decrypted else "✗ Mismatch"
            print(f"  Verify    : {match}")

        elif choice == '4':
            text = input("\n  Enter ciphertext to brute-force : ")
            brute_force(text)

        elif choice == '5':
            print("\nGoodbye!")
            break

        else:
            print("  Invalid choice — please enter 1 to 5.")


if __name__ == "__main__":
    main()
