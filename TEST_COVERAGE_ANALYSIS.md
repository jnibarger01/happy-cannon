# Test Coverage Analysis - Jace Pay Calculator

**Date**: January 5, 2026
**Project**: Hendrick Toyota Merriam - Service Advisor Pay Calculator
**Current Test Coverage**: 0% (No automated tests)

## Executive Summary

This codebase currently has **zero automated tests**. The application is a single-page vanilla JavaScript application with approximately **710 lines of JavaScript code** across 6 distinct modules. All testing is currently manual, which creates significant risk for:

- Regression bugs when making changes
- Incorrect commission/bonus calculations affecting employee pay
- Data corruption or loss issues
- Cross-browser compatibility problems
- Edge case handling failures

## Project Structure

```
happy-cannon/
├── index.html          # 1,096 lines (HTML + CSS + JavaScript)
├── package.json        # No dependencies
└── Documentation files
```

**JavaScript Modules** (all in index.html, lines 377-1094):
1. **Calculator** (lines 381-479) - 98 lines
2. **Storage** (lines 484-552) - 68 lines
3. **StateManager** (lines 557-585) - 28 lines
4. **UI** (lines 590-716) - 126 lines
5. **Events** (lines 721-977) - 256 lines
6. **Tabs** (lines 982-1008) - 26 lines
7. **Init** (lines 1013-1094) - 81 lines

---

## Critical Areas Requiring Test Coverage

### 🔴 CRITICAL PRIORITY

#### 1. Calculator Module (lines 381-479)
**Risk Level**: CRITICAL - Financial calculations affecting employee compensation

**Missing Tests**:
- `calcMonthlyCommission()` - Commission calculation logic
  - CP Net calculation: `(cpLabor + cpParts - discounts) * 7%`
  - Warranty calculation: `(wLabor + wParts) * 5%`
  - Internal calculation: `(iLabor + iParts) * 5%`
  - Sublet calculation: `subletLabor * 0.5%`
  - Edge case: Negative discounts exceeding CP total
  - Edge case: Zero values
  - Edge case: Very large numbers (>$1M)

- `calcTierBonus()` - Tier bonus calculation
  - Tier thresholds: $35K, $45K, $55K, $65K
  - Each tier adds 0.5% bonus
  - Eligibility gating (7+ reviews required)
  - Edge case: CP Net exactly at threshold
  - Edge case: CP Net just below threshold ($34,999.99)
  - Edge case: Ineligible user with high CP Net

- `calcObjectiveBonus()` - Objective bonus calculation
  - Weighted mode: 6 different objective weights
  - Manual mode: Custom percentage input
  - Max cap: 1.0% (0.01)
  - Edge case: All objectives checked
  - Edge case: Invalid manual percentage (negative, >1.0)
  - Edge case: Float precision issues

- `calcReviewBonus()` - Review bonus calculation
  - Threshold: 15 reviews = $375
  - Edge case: Exactly 15 reviews
  - Edge case: 14 reviews (should be $0)
  - Edge case: Negative review count

**Recommended Tests**: 40+ unit tests

**Example Test Cases**:
```javascript
// Sample test structure (not actual code)
describe('Calculator.calcMonthlyCommission', () => {
  it('should calculate CP commission correctly', () => {
    const result = Calculator.calcMonthlyCommission({
      cpLabor: 10000, cpParts: 5000, discounts: 500,
      wLabor: 0, wParts: 0, iLabor: 0, iParts: 0,
      subletLabor: 0, hoursSold: 0
    });
    // Expected: (10000 + 5000 - 500) * 0.07 = $1,015
    expect(result.commission).toBe(1015);
  });

  it('should handle discounts exceeding CP total', () => {
    const result = Calculator.calcMonthlyCommission({
      cpLabor: 1000, cpParts: 0, discounts: 2000,
      // ... other fields
    });
    // CP Net should be max(0, ...) = 0
    expect(result.cpNet).toBe(0);
    expect(result.commission).toBeGreaterThanOrEqual(0);
  });
});
```

---

#### 2. Storage Module (lines 484-552)
**Risk Level**: CRITICAL - Data persistence and potential data loss

**Missing Tests**:
- `save()` - localStorage write operations
  - Test successful save
  - Test localStorage quota exceeded error
  - Test private/incognito mode (no localStorage)

- `load()` - localStorage read operations
  - Test successful load
  - Test corrupted JSON parsing
  - Test missing data (first-time user)
  - Test version migration (future compatibility)
  - Edge case: Empty localStorage
  - Edge case: Malformed JSON

- `createDefaultState()` - Default state generation
  - Verify structure matches expected schema
  - Verify all required fields present

- Data integrity
  - Round-trip test: save → load → verify equality
  - Test with special characters in month labels
  - Test with very large datasets (100+ months)

**Recommended Tests**: 20+ unit tests

---

### 🟡 HIGH PRIORITY

#### 3. StateManager Module (lines 557-585)
**Risk Level**: HIGH - State synchronization bugs

**Missing Tests**:
- `init()` - Initialization
- `get()` - State retrieval
- `update()` - State updates and storage sync
  - Test state properly updated
  - Test storage automatically saved
  - Test subscribers notified

- `subscribe()` / `notify()` - Pub/sub pattern
  - Test multiple subscribers receive notifications
  - Test unsubscribe works correctly
  - Test notification order
  - Edge case: Subscribe during notification
  - Edge case: Unsubscribe during notification

- State immutability
  - Verify updates don't mutate original state
  - Test concurrent updates

**Recommended Tests**: 15+ unit tests

---

#### 4. UI Formatting Module (lines 590-716)
**Risk Level**: HIGH - Display bugs and XSS vulnerabilities

**Missing Tests**:
- `money()` - Currency formatting
  - Test positive numbers
  - Test zero
  - Test negative numbers (should show $-1,000)
  - Test decimal precision
  - Test large numbers (millions)
  - Edge case: NaN, null, undefined inputs

- `moneyExact()` - Exact currency formatting
  - Test cent precision ($1,234.56)

- `pct()` - Percentage formatting
  - Test 0%, 50%, 100%
  - Test decimal percentages (7.5%)
  - Test >100% (goal progress)

- `escape()` - HTML escaping (XSS prevention)
  - Test `<script>` tags escaped
  - Test `&`, `<`, `>`, `"` characters
  - Test empty strings
  - **SECURITY**: Prevent XSS injection

- `renderGoalProgress()` - Goal bar rendering
  - Test 0% progress
  - Test 50% progress
  - Test 100% progress
  - Test >100% progress (cap at 100%?)
  - Test goal bar width calculation

- `renderCommissionSummary()` - Commission display
  - Test with zero sales
  - Test with mixed sales types
  - Test breakdown text generation

- `renderMonthlyBonus()` - Bonus display
  - Test tier pills (eligible/ineligible)
  - Test review bonus pills
  - Test tier breakdown rendering

**Recommended Tests**: 30+ unit tests

---

### 🟢 MEDIUM PRIORITY

#### 5. Events Module (lines 721-977)
**Risk Level**: MEDIUM - User interaction bugs

**Missing Tests**:
- `loadMonth()` - Month loading
  - Test loading existing month
  - Test loading non-existent month
  - Test without selecting month (alert)
  - Test input hydration

- `newMonth()` - Month creation
  - Test creating new month
  - Test duplicate month warning
  - Test user cancels confirmation

- `saveMonth()` - Month saving
  - Test saving with no current month (alert)
  - Test number parsing from inputs
  - Test state update

- `deleteMonth()` - Month deletion
  - Test successful deletion
  - Test cancellation
  - Test deleting non-existent month

- `exportData()` - JSON export
  - Test file download triggers
  - Test filename format
  - Test JSON structure

- `importData()` / `handleFileImport()` - JSON import
  - Test valid JSON import
  - Test invalid JSON (parse error)
  - Test missing required fields
  - Test user cancels confirmation
  - Test malformed file

- `saveMonthlyBonus()` - Bonus data saving
  - Test checkbox state capture
  - Test number parsing

- `wipeAll()` - Data wipe
  - Test confirmation required
  - Test complete data deletion

**Recommended Tests**: 35+ unit tests

---

#### 6. Tab Navigation (lines 982-1008)
**Risk Level**: LOW - UI state bugs

**Missing Tests**:
- `showTab()` - Tab switching
  - Test correct tab shown
  - Test other tabs hidden
  - Test active class applied
  - Test bonus inputs hydrated on bonuses tab

**Recommended Tests**: 5+ unit tests

---

#### 7. Initialization (lines 1013-1094)
**Risk Level**: MEDIUM - App startup issues

**Missing Tests**:
- `init()` - Application initialization
  - Test event listeners attached
  - Test current month set to today
  - Test state loaded from storage
  - Test initial render
  - Test auto-save listeners attached
  - Test StateManager subscription

**Recommended Tests**: 10+ unit tests (integration-style)

---

## Edge Cases & Error Scenarios

### Data Validation Issues
- [ ] Non-numeric input in number fields
- [ ] Negative numbers where invalid
- [ ] Extremely large numbers (>$1B)
- [ ] Float precision issues (0.1 + 0.2 ≠ 0.3)
- [ ] Empty strings in numeric fields

### Browser Compatibility
- [ ] localStorage disabled/unavailable
- [ ] Private/incognito mode
- [ ] localStorage quota exceeded
- [ ] Different browser number formatting

### User Flow Issues
- [ ] Rapid clicks on save button
- [ ] Navigating away during save
- [ ] Multiple tabs open (state sync)
- [ ] Browser back/forward buttons
- [ ] Page refresh during input

### Data Integrity
- [ ] Import file from older version
- [ ] Import file from newer version
- [ ] Corrupted localStorage data
- [ ] Race conditions in state updates

---

## Recommended Testing Stack

Since this is a vanilla JavaScript project with no build system, consider:

### Option 1: Vitest (Recommended)
**Pros**: Modern, fast, minimal config, great DX
**Setup**:
```json
{
  "devDependencies": {
    "vitest": "^1.0.0",
    "jsdom": "^23.0.0"
  }
}
```

### Option 2: Jest
**Pros**: Mature, widely adopted, great docs
**Cons**: Slightly heavier than Vitest

### Option 3: Playwright (E2E)
**Pros**: Real browser testing, catches integration bugs
**Cons**: Slower, more complex

### Recommended Approach: **Vitest + Playwright**
- Vitest for unit tests (Calculator, Storage, StateManager, UI)
- Playwright for E2E tests (full user flows)

---

## Test Implementation Roadmap

### Phase 1: Critical Tests (Week 1-2)
**Goal**: Protect financial calculations

1. Extract Calculator module to separate file
2. Write 40+ Calculator unit tests
3. Write 20+ Storage unit tests
4. Set up CI/CD to run tests on every commit

**Risk Mitigation**: Prevents incorrect pay calculations

---

### Phase 2: High Priority (Week 3-4)
**Goal**: Prevent state/UI bugs

1. Extract StateManager to separate file
2. Write 15+ StateManager tests
3. Write 30+ UI formatting tests (including XSS tests)

**Risk Mitigation**: Prevents data corruption and display bugs

---

### Phase 3: Medium Priority (Week 5-6)
**Goal**: Cover user interactions

1. Set up Playwright for E2E testing
2. Write 35+ Events module tests
3. Write 5+ E2E user flow tests:
   - Create month → Enter data → Save → Export
   - Import data → Verify loaded correctly
   - Calculate bonus → Verify tiers → Save

**Risk Mitigation**: Prevents UI workflow bugs

---

### Phase 4: Refactoring (Week 7+)
**Goal**: Improve testability

1. Extract all modules to separate files
2. Add TypeScript for type safety
3. Achieve 90%+ code coverage
4. Add property-based testing for Calculator

---

## Specific Test Cases by Module

### Calculator Tests (40 tests)

#### calcMonthlyCommission (15 tests)
1. ✅ Zero values → $0 commission
2. ✅ CP only → 7% commission
3. ✅ Warranty only → 5% commission
4. ✅ Internal only → 5% commission
5. ✅ Sublet only → 0.5% commission
6. ✅ Mixed sales → Correct total
7. ✅ Discounts reduce CP net
8. ✅ Discounts > CP total → CP net = $0
9. ✅ Large numbers (>$1M)
10. ✅ Float precision (e.g., $10.01 + $20.02)
11. ✅ Negative discounts (invalid input)
12. ✅ All fields maxed out
13. ✅ Hours sold tracked separately
14. ✅ Return values structure correct
15. ✅ No mutation of input data

#### calcTierBonus (12 tests)
1. ✅ Ineligible → $0 bonus
2. ✅ Eligible but below $35K → $0 bonus
3. ✅ Exactly $35K → 0.5% bonus
4. ✅ Exactly $45K → 1.0% bonus
5. ✅ Exactly $55K → 1.5% bonus
6. ✅ Exactly $65K → 2.0% bonus
7. ✅ $100K (all tiers) → 2.0% bonus
8. ✅ $34,999.99 → $0 bonus
9. ✅ $35,000.01 → 0.5% bonus
10. ✅ Tier labels returned correctly
11. ✅ Dollar calculation matches percentage
12. ✅ Eligible flag properly gates bonus

#### calcObjectiveBonus (8 tests)
1. ✅ Weighted mode, no objectives → 0%
2. ✅ Weighted mode, all objectives → 1.0%
3. ✅ Weighted mode, partial objectives
4. ✅ Manual mode with valid percentage
5. ✅ Manual mode >1.0 → capped at 1.0%
6. ✅ Manual mode <0 → capped at 0%
7. ✅ Dollar calculation correct
8. ✅ Mode switching works

#### calcReviewBonus (5 tests)
1. ✅ 0 reviews → $0
2. ✅ 14 reviews → $0
3. ✅ 15 reviews → $375
4. ✅ 20 reviews → $375
5. ✅ Negative reviews → $0

---

### Storage Tests (20 tests)

#### save/load (10 tests)
1. ✅ Save → Load → Data matches
2. ✅ Load with no data → Default state
3. ✅ Load with corrupted JSON → Default state
4. ✅ Save to full localStorage → Handle error
5. ✅ Save in private mode → Handle error
6. ✅ Load logs error on failure
7. ✅ Multiple months saved correctly
8. ✅ Bonus data persisted correctly
9. ✅ currentMonthId persisted
10. ✅ Special characters in month labels

#### Defaults (5 tests)
1. ✅ createDefaultState structure
2. ✅ createDefaultMonthData structure
3. ✅ createDefaultBonusData structure
4. ✅ All numeric fields default to 0
5. ✅ All objects/arrays properly initialized

#### Edge Cases (5 tests)
1. ✅ Very large dataset (100+ months)
2. ✅ Empty string keys
3. ✅ Unicode characters in data
4. ✅ null/undefined values sanitized
5. ✅ wipe() clears all data

---

### StateManager Tests (15 tests)

1. ✅ init() loads from storage
2. ✅ get() returns current state
3. ✅ update() modifies state
4. ✅ update() saves to storage
5. ✅ update() notifies subscribers
6. ✅ subscribe() adds listener
7. ✅ Unsubscribe removes listener
8. ✅ Multiple subscribers all notified
9. ✅ Notification order preserved
10. ✅ update() doesn't mutate input
11. ✅ Subscriber receives new state
12. ✅ Unsubscribe during notification
13. ✅ Subscribe during notification
14. ✅ Error in subscriber doesn't break others
15. ✅ State persists across init cycles

---

### UI Tests (30 tests)

#### Formatting (12 tests)
1. ✅ money(1234) → "$1,234"
2. ✅ money(0) → "$0"
3. ✅ money(-500) → "$-500"
4. ✅ money(1000000) → "$1,000,000"
5. ✅ moneyExact(1234.56) → "$1,234.56"
6. ✅ pct(0.075) → "7.5%"
7. ✅ pct(0) → "0.0%"
8. ✅ pct(1.5) → "150.0%"
9. ✅ escape("<script>") → "&lt;script&gt;"
10. ✅ escape("&") → "&amp;"
11. ✅ money(null) → "$0"
12. ✅ money(undefined) → "$0"

#### Rendering (18 tests)
1. ✅ renderGoalProgress at 0%
2. ✅ renderGoalProgress at 50%
3. ✅ renderGoalProgress at 100%
4. ✅ renderGoalProgress >100% capped
5. ✅ renderGoalProgress updates DOM
6. ✅ renderCommissionSummary with zero data
7. ✅ renderCommissionSummary with CP only
8. ✅ renderCommissionSummary with mixed sales
9. ✅ Breakdown shows only non-zero items
10. ✅ Breakdown calculates percentages correctly
11. ✅ renderMonthlyBonus with eligible tier
12. ✅ renderMonthlyBonus with ineligible tier
13. ✅ Review bonus pill at 15 reviews
14. ✅ Review bonus pill at <15 reviews
15. ✅ Tier breakdown shows all 4 tiers
16. ✅ Tier checkmarks for hit tiers
17. ✅ renderMonthsList with multiple months
18. ✅ renderMonthsList highlights current month

---

## Security Considerations

### XSS Vulnerabilities
**Risk**: Month labels and imported data could contain malicious HTML

**Tests Needed**:
```javascript
it('should escape HTML in month labels', () => {
  const html = UI.escape('<img src=x onerror=alert(1)>');
  expect(html).not.toContain('<');
  expect(html).toContain('&lt;');
});
```

### localStorage Attacks
**Risk**: User could manually edit localStorage to inject invalid data

**Tests Needed**:
- Validate all numeric fields are actually numbers
- Sanitize string inputs
- Verify imported JSON structure

---

## Performance Considerations

### Current Issues
- No debouncing on auto-save inputs
- Re-renders entire month list on every change
- No memoization of calculations

### Performance Tests Needed
1. Test auto-save doesn't trigger on every keystroke
2. Test large datasets (100+ months) render performance
3. Test calculation performance with extreme values

---

## Browser Compatibility Testing

### Minimum Test Matrix
- ✅ Chrome (latest)
- ✅ Firefox (latest)
- ✅ Safari (latest)
- ✅ Edge (latest)
- ✅ Mobile Safari (iOS)
- ✅ Chrome Mobile (Android)

### Specific Issues to Test
- Number input formatting differences
- localStorage implementation differences
- Date input support (month picker)
- File download/upload APIs

---

## Metrics & Goals

### Coverage Goals
- **Unit Test Coverage**: 90%+
- **Integration Test Coverage**: 80%+
- **E2E Test Coverage**: Critical user flows (5-10 tests)

### Quality Gates (CI/CD)
- [ ] All tests must pass before merge
- [ ] No decrease in code coverage
- [ ] No console errors during tests
- [ ] Performance benchmarks met

### Success Metrics
- **0 regression bugs** in production after test implementation
- **<5 minutes** to run full test suite
- **<30 seconds** to run unit tests only
- **100% confidence** in calculation accuracy

---

## Estimated Effort

| Phase | Tests | Effort | Priority |
|-------|-------|--------|----------|
| Calculator | 40 | 16 hours | CRITICAL |
| Storage | 20 | 8 hours | CRITICAL |
| StateManager | 15 | 6 hours | HIGH |
| UI | 30 | 12 hours | HIGH |
| Events | 35 | 14 hours | MEDIUM |
| Tabs | 5 | 2 hours | LOW |
| Init | 10 | 4 hours | MEDIUM |
| E2E | 10 | 16 hours | HIGH |
| **TOTAL** | **165** | **78 hours** | — |

---

## Next Steps

1. **Set up testing infrastructure** (Vitest + Playwright)
2. **Extract modules** to separate files for better testability
3. **Start with Calculator tests** (highest risk)
4. **Add CI/CD** to run tests automatically
5. **Gradually increase coverage** following the roadmap
6. **Refactor for testability** as needed

---

## Conclusion

This codebase handles **financial calculations that directly affect employee compensation**. The lack of automated tests represents a significant business risk. A comprehensive test suite would:

- ✅ Prevent pay calculation errors
- ✅ Enable safe refactoring and feature additions
- ✅ Catch regressions before production
- ✅ Document expected behavior
- ✅ Improve developer confidence

**Recommended Action**: Implement Phase 1 (Calculator + Storage tests) immediately to protect the most critical functionality.
