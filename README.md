Here's a clean and structured Markdown version of your text, formatted for clarity and presentation:

---

# Solving Sudoku Puzzles: A Comparative Study of Backtracking, Reinforcement Learning, and MRV Heuristic  
**Course**: AI 801 - Foundations of Artificial Intelligence  
**Team Members**: Youngmin Yoo, Yugandhar Gopu  

---

## 1. Introduction  
Sudoku is a combinatorial logic puzzle played on a 9x9 grid subdivided into 3x3 blocks. The goal is to fill the grid so that each row, column, and 3x3 block contains all the digits from 1 to 9 without repetition. Sudoku poses challenges due to its constraints and large search space.

### Approaches Explored  
1. **Backtracking Algorithm** – A classical recursive method for constraint satisfaction problems.  
2. **Reinforcement Learning (Q-Learning)** – A machine learning approach using state-action pair learning.  
3. **MRV (Minimum-Remaining-Values)** – A heuristic optimization that improves decision-making efficiency.  

### Goals of the Study  
- Compare the performance of classical algorithms with AI-based methods.  
- Measure efficiency in terms of step count and success rates.  
- Analyze the impact of the MRV heuristic on both approaches.  

---

## 2. Problem Definition  

### 2.1 Rules of Sudoku  
The Sudoku grid follows three constraints:  
1. Each row must contain numbers 1 to 9 without repetition.  
2. Each column must contain numbers 1 to 9 without repetition.  
3. Each 3x3 subgrid must contain numbers 1 to 9 exactly once.  

### 2.2 Dataset  
Dataset used: **“9 Million Sudoku Puzzles and Solutions”** from Kaggle. Puzzles were selected based on difficulty:  
- **Easy**: Fewer blank cells (3 puzzles).  
- **Medium**: Moderate number of blank cells (3 puzzles).  
- **Hard**: Many blank cells (3 puzzles).  

---

## 3. Methodologies  

### 3.1 Backtracking Algorithm  
The backtracking algorithm attempts to fill each empty cell with valid candidates. If a conflict arises, the algorithm backtracks and explores alternative candidates.  

#### Code Snippet: Backtracking Algorithm  
```python
def solve_sudoku(s, step_count=0):
    empty_cell = find_empty_cell(s)
    if empty_cell is None:
        return True, step_count  # Puzzle solved
    r, c = empty_cell
    candidates = s.find_possible_values(r, c)
    for val in candidates:
        act = action(r, c, val)
        if s.constrain_check(act):
            s.apply_action(act)
            solved, final_steps = solve_sudoku(s, step_count + 1)
            if solved:
                return True, final_steps
        s.revert_action(act)  # Backtrack
    return False, step_count
```

### 3.2 MRV Heuristic  
MRV selects the cell with the minimum remaining candidate values, reducing the search space early.  

#### MRV Integration  
```python
def find_empty_cell(s):
    min_candidates = 10
    candidates_cells = []
    for r in range(9):
        for c in range(9):
            if s.board[r][c] == 0:
                candidates = s.find_possible_values(r, c)
                if len(candidates) < min_candidates:
                    min_candidates = len(candidates)
                    candidates_cells = [(r, c)]
    return candidates_cells[0] if candidates_cells else None
```

### 3.3 Reinforcement Learning (Q-Learning)  
Q-Learning uses a Q-Table to estimate the quality of state-action pairs. It learns iteratively through rewards and penalties.  

#### Q-Learning Framework  
```python
def update_Q(old_s, a, reward, new_s, done):
    old_sk = encode_state(old_s)
    ak = (a.row, a.col, a.val)
    old_q = get_Q(old_sk, ak)
    if done:
        target = reward
    else:
        new_sk = encode_state(new_s)
        next_qs = [get_Q(new_sk, (na.row, na.col, na.val)) for na in get_possible_actions(new_s)]
        target = reward + gamma * max(next_qs)
    Q[(old_sk, ak)] = old_q + alpha * (target - old_q)
```

---

## 4. Experimental Setup  
- **Algorithms**: Backtracking (with/without MRV), Q-Learning (with/without MRV).  
- **Metrics**:  
  - Step count to convergence.  
  - Success rate (% of puzzles solved).  
- **Puzzles**: 9 Sudoku puzzles (3 easy, 3 medium, 3 hard).  

---

## 5. Results  

### 5.1 Backtracking Results  

| Puzzle | Steps (MRV Disabled) | Steps (MRV Enabled) | Solved |  
|--------|-----------------------|---------------------|--------|  
| 1      | 24                    | 24                  | Yes    |  
| 2      | 24                    | 24                  | Yes    |  
| 3      | 23                    | 23                  | Yes    |  
| 4      | 43                    | 43                  | Yes    |  
| 5      | 42                    | 42                  | Yes    |  
| 6      | 43                    | 43                  | Yes    |  
| 7      | 47                    | 47                  | Yes    |  
| 8      | 48                    | 48                  | Yes    |  
| 9      | 48                    | 48                  | Yes    |  

### 5.2 Q-Learning Results  

| Puzzle | Success Rate (Disabled) | Success Rate (Enabled) | Steps (Enabled) |  
|--------|--------------------------|-------------------------|------------------|  
| 1      | 0%                       | 100%                   | 240              |  
| 2      | 0%                       | 100%                   | 240              |  
| 3      | 20%                      | 100%                   | 230              |  
| 4      | 0%                       | 100%                   | 430              |  
| 5      | 0%                       | 10%                    | 395              |  
| 6      | 0%                       | 100%                   | 430              |  
| 7      | 0%                       | 100%                   | 470              |  
| 8      | 0%                       | 100%                   | 480              |  
| 9      | 0%                       | 60%                    | 465              |  

---

## 6. Analysis  

### 6.1 Backtracking Performance  
- **Efficiency**: Highly efficient, solving all puzzles reliably.  
- **MRV**: Marginally reduced step count but did not impact solvability.  

### 6.2 Q-Learning Performance  
- **Without MRV**: RL failed to converge on most puzzles.  
- **With MRV**: Success rates significantly improved. However, training time remains a major limitation.  

### 6.3 Comparative Analysis  

| Criterion      | Backtracking | Q-Learning       |  
|----------------|--------------|------------------|  
| Efficiency     | High         | Low              |  
| Reliability    | Consistent   | Inconsistent     |  
| Training       | None         | Extensive        |  

---

## 7. Conclusion  
This study compared classical and AI-based methods for solving Sudoku:  
1. **Backtracking Algorithm**: Simple, efficient, and reliable, making it the gold standard.  
2. **Reinforcement Learning**: Showed promise when integrated with the MRV heuristic but required substantial training and computational resources.  
3. **MRV Heuristic**: Crucial for reducing search space and improving Q-Learning convergence.  

