# Privacy-Preserving AI Bounty Judge

## Overview

This project implements a Commit-Reveal bounty system.

Participants first submit a commitment hash instead of their actual answer.

After the submission deadline, they reveal their answer together with a secret salt.

The contract verifies the commitment before allowing the answer to be judged by AI.

## Workflow

1. Create Bounty
2. Submit Commitment
3. Reveal Answer
4. AI Judge
5. Finalize Winner