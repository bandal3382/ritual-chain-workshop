# Privacy-Preserving AI Bounty Judge

## Overview

This project implements a privacy-preserving bounty system using a Commit-Reveal scheme.

The original bounty system exposed submissions publicly, allowing participants to copy and improve existing answers. This implementation keeps answers hidden during the submission phase by storing only commitment hashes.

Participants first submit a commitment hash generated from:

keccak256(answer, salt, msg.sender, bountyId)

After the submission phase, participants reveal their answer and salt. The smart contract verifies the commitment before allowing the answer to be considered for AI judging.

## Workflow

1. Create Bounty
   - A bounty owner creates a bounty with a reward.

2. Submit Commitment
   - Participants submit only a commitment hash.
   - The actual answer remains private.

3. Reveal Answer
   - Participants reveal their answer and salt.
   - The contract verifies the hash matches the original commitment.

4. AI Judge
   - Only valid revealed submissions are eligible for AI evaluation.

5. Finalize Winner
   - The bounty owner selects the winner and transfers the reward.

## Smart Contract Functions

### submitCommitment()

Participants submit:

- bountyId
- commitment hash

The contract stores the commitment without revealing the answer.

### revealAnswer()

Participants reveal:

- answer
- salt

The contract checks:

keccak256(answer, salt, msg.sender, bountyId)

against the stored commitment.

### judgeAll()

Runs AI judging on revealed submissions.

### finalizeWinner()

Finalizes the winning submission and transfers the reward.

## Test Plan

### Valid Reveal

- Submit a commitment.
- Reveal with the correct answer and salt.
- Expected result: reveal succeeds.

### Invalid Reveal

- Submit a commitment.
- Reveal with incorrect salt or answer.
- Expected result: transaction fails.

### Double Reveal

- Reveal the same submission twice.
- Expected result: transaction fails.

### Judge Before Reveal

- Attempt AI judging before any valid reveal.
- Expected result: transaction fails.

## Architecture Note

The blockchain stores bounty information, commitment hashes, and verified revealed answers.

Before reveal, plaintext answers exist only with participants and are not stored on-chain.

After reveal, the verified answers become available for AI judging.

The AI system evaluates submissions after verification, while final winner selection remains controlled by the bounty owner.

## Reflection

In a bounty system, public information should include bounty rules, reward amounts, and judging criteria. Private information should include participant answers before the reveal phase. Commitment hashes protect submissions from being copied by other participants. AI should evaluate objective factors such as quality and relevance. Human decisions should remain for subjective judgment and final approval. A fair bounty system requires both transparency and privacy.