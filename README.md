
LAB WORKBENCH: CUSTOM BANNED PASSWORD DICTIONARY ENFORCEMENT
================================================================================

1. PRE-DEPLOYMENT TARGET IDENTIFICATION
--------------------------------------------------------------------------------
To test this enforcement mechanism accurately without locked-out states, we will 
target the same directory test user profiles from our active environment:

* Alpha Engineer (Source): Used to verify successful standard password changes.
* Bravo Engineer (Source): Used to verify the explicit custom block execution.
* Selected Custom Keyword: "ProjectQuantum" (Case-insensitive, substring matched)

2. COMPONENT DEPLOYMENT & PORTAL PATHS
--------------------------------------------------------------------------------
Execute the directory configurations precisely using the following steps within 
the Microsoft Entra admin center:

Step 1: Navigate to the Protection Interface
  - Open the Microsoft Entra ID (MEID) portal.
  - In the left-hand navigation pane, expand the "Protection" dropdown section.
  - Click directly on "Authentication methods".
  - Under the "Manage" sub-header, click on "Password protection".

Step 2: Configure the Global Custom Dictionary Settings
  - Locate the setting labeled "Enforce custom list".
  - Switch the radio toggle button from "No" to "Yes".
  - Locate the text box labeled "Custom password list".
  - Enter the string "ProjectQuantum" into the list (one entry per line).
  
Step 3: Configure the Lockout Thresholds (Smart Lockout baseline)
  - Verify "Lockout threshold" is set to your tenant baseline (Default: 10).
  - Verify "Lockout duration in seconds" is set to baseline (Default: 60).
  - Leave "Mode" set to "Enforce" to ensure live blocking is active.

Step 4: Commit and Save the Configuration
  - Click the "Save" button at the top menu bar of the blade.
  - Wait 60 seconds for the global policy configuration to write to the tenant.

3. EXPECTED OPERATIONAL HANDSHAKE & INVERSE LOGIC
--------------------------------------------------------------------------------
The Microsoft Entra Password Protection (MEPP) evaluation engine uses a token-
parsing logic model when a user initiates a password change:

* The Failure State (Bravo Change Blocked): Bravo Engineer logs into the self-
  service profile page (https://myaccount.microsoft.com) and attempts to update 
  their credential. Bravo inputs a password containing the substring, such as 
  "ProjectQuantum123!". The MEPP engine parses the text string, identifies the 
  matching custom token, and immediately triggers a hard boolean failure state. 
  The engine completely rejects the write operation and displays a portal error 
  stating the password contains a banned word.

* The Success State (Alpha Change Allowed): Alpha Engineer logs into the self-
  service profile page and updates their credential to a strong, independent 
  value like "X7#mK9!vP2$qL5". The MEPP engine evaluates the string against both 
  the global and custom banned lists. Finding no token overlaps, the engine 
  renders a hard boolean success state, commits the change to the directory, 
  and issues a fresh authentication token state.

4. LIVE VERIFICATION EXECUTION PLAN
--------------------------------------------------------------------------------
Execute these verification steps immediately after the propagation window:

1. Open a fresh private Incognito browser window.
2. Navigate to https://myaccount.microsoft.com and sign in as Bravo Engineer.
3. Click "Change password" on the Security Info card.
4. Attempt to change the password to: ProjectQuantum2026!
5. Confirm the engine throws an explicit error blocking the update.
6. Close the session, open a new private window, and sign in as Alpha Engineer.
7. Attempt to change the password to a standard unique baseline string.
8. Confirm the change completes successfully with no directory friction.
================================================================================



LAB COMPLETION SUMMARY: CUSTOM BANNED PASSWORD DICTIONARY ENFORCEMENT
================================================================================

1. VERIFICATION RESULTS
--------------------------------------------------------------------------------
* Bravo Engineer (Source): Attempted a credential update to a string containing 
  the forbidden keyword "ProjectQuantum". The Microsoft Entra Password Protection 
  (MEPP) engine successfully executed a hard boolean block, rejecting the directory 
  write operation and returning error code 120018 ("We've seen that password 
  too many times before"). This confirms the policy trap is active and functional.
      
* Alpha Engineer (Source): Attempted a credential update to a unique, highly 
  complex baseline string completely free of the custom dictionary words. The 
  MEPP engine parsed the string, validated compliance, and permitted a successful 
  directory update without friction.

2. ARCHITECTURAL CONCLUSION
--------------------------------------------------------------------------------
The deployment successfully verifies that the Microsoft Entra ID (MEID) credential 
validation gateway can actively parse and reject specific organizational vocabulary 
terms. By turning on custom list enforcement, the tenant now dynamically blocks 
predictable corporate passwords during self-service updates, establishing a clear 
and automated identity-plane defense.
================================================================================
