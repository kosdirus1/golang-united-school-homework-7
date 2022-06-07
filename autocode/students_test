package coverage

import (
	"os"
	"strings"
	"testing"
	"time"
)

// DO NOT EDIT THIS FUNCTION
func init() {
	content, err := os.ReadFile("students_test.go")
	if err != nil {
		panic(err)
	}
	err = os.WriteFile("autocode/students_test", content, 0644)
	if err != nil {
		panic(err)
	}
}

// WRITE YOUR CODE BELOW
func TestPeople_Len(t *testing.T) {
	p := People{Person{
		firstName: "Bob",
		lastName:  "Kendrick",
		birthDay: time.Date(2003, 05, 20, 0, 0,
			0, 0, time.UTC),
	}, Person{
		firstName: "Ken",
		lastName:  "Bobrick",
		birthDay: time.Date(1984, 12, 31, 0, 0,
			0, 0, time.UTC),
	}}
	pExpect := len(p)
	pRes := p.Len()
	if pRes != pExpect {
		t.Errorf("Expected: %d, got %d", pExpect, pRes)
	}
}

func TestPeople_Less(t *testing.T) {
	p := People{Person{
		firstName: "Bob",
		lastName:  "Kendrick",
		birthDay: time.Date(2003, 05, 20, 0, 0,
			0, 0, time.UTC),
	}, Person{
		firstName: "Ken",
		lastName:  "Bobrick",
		birthDay: time.Date(1984, 12, 31, 0, 0,
			0, 0, time.UTC),
	}, Person{
		firstName: "Ken",
		lastName:  "Dobrick",
		birthDay: time.Date(1984, 12, 31, 0, 0,
			0, 0, time.UTC),
	}, Person{
		firstName: "Bil",
		lastName:  "Clinton",
		birthDay: time.Date(1984, 12, 31, 0, 0,
			0, 0, time.UTC),
	}}
	tData := map[string]struct {
		i        int
		j        int
		expected bool
	}{
		"date comparison":      {i: 0, j: 1, expected: true},
		"firstName comparison": {i: 1, j: 3, expected: false},
		"lastName comparison":  {i: 1, j: 2, expected: true},
	}
	for name, tcase := range tData {
		v := tcase
		t.Run(name, func(t *testing.T) {
			got := p.Less(v.i, v.j)
			if got != v.expected {
				t.Errorf("[%s] expected: %v, got %v", name, v.expected, got)
			}
		})
	}
}

func TestPeople_Swap(t *testing.T) {
	p := People{Person{
		firstName: "Bob",
		lastName:  "Kendrick",
		birthDay: time.Date(2003, 05, 20, 0, 0,
			0, 0, time.UTC),
	}, Person{
		firstName: "Ken",
		lastName:  "Bobrick",
		birthDay: time.Date(1984, 12, 31, 0, 0,
			0, 0, time.UTC),
	}}
	p0 := p[0]
	p1 := p[1]
	p.Swap(0, 1)
	if p[0] != p1 || p[1] != p0 {
		t.Errorf("expected: 1. %v, 2. %v; got: 1. %v, 2. %v", p1, p0, p[0], p[1])
	}
}

func TestNew(t *testing.T) {
	tData := map[string]struct {
		str            string
		expectedMatrix *Matrix
		err            string
	}{
		"invalid syntax": {str: "1.1 1.2 1.3\n2.1 2.2 \n3.1 3.2 " +
			"3.3 3.4", expectedMatrix: nil, err: "strconv.Atoi: parsing \"1.1\": invalid syntax"},
		"success": {str: "1 2 3 \n 4 5 6 \n 7 8 9", expectedMatrix: &Matrix{
			rows: 3,
			cols: 3,
			data: []int{1, 2, 3, 4, 5, 6, 7, 8, 9},
		}, err: ""},
		"invalid number of cols": {str: "1 2 3 \n 4 5 5 6 \n 7 8 9", expectedMatrix: nil,
			err: "Rows need to be the same length"},
	}
	for name, tcase := range tData {
		v := tcase
		t.Run(name, func(t *testing.T) {
			matrix, err := New(v.str)

			if err != nil && !strings.Contains(err.Error(), v.err) {
				t.Errorf("[%s] error happend while not expected: %s", name, err.Error())
			} else if err == nil && (matrix.cols != v.expectedMatrix.cols ||
				matrix.rows != v.expectedMatrix.rows || !sliceEqual(matrix.data, v.expectedMatrix.data)) {
				t.Errorf("[%s] mistake in matrix, expected: %v, received: %v", name, matrix, v.expectedMatrix)
			}
		})
	}
}

func sliceEqual(a, b []int) bool {
	if len(a) != len(b) {
		return false
	}
	for i, v := range a {
		if v != b[i] {
			return false
		}
	}
	return true
}

func TestMatrix_Rows(t *testing.T) {
	expected := [][]int{{1, 2, 3}, {4, 5, 6}, {7, 8, 9}}
	m := &Matrix{
		rows: 3,
		cols: 3,
		data: []int{1, 2, 3, 4, 5, 6, 7, 8, 9}}
	res := m.Rows()
	for i := range res {
		if !sliceEqual(res[i], expected[i]) {
			t.Errorf("expected: %v, got: %v. Index: %d", expected[i], res[i], i)
		}
	}
}

func TestMatrix_Cols(t *testing.T) {
	expected := [][]int{{1, 4, 7}, {2, 5, 8}, {3, 6, 9}}
	m := &Matrix{
		rows: 3,
		cols: 3,
		data: []int{1, 2, 3, 4, 5, 6, 7, 8, 9}}
	res := m.Cols()
	for i := range res {
		if !sliceEqual(res[i], expected[i]) {
			t.Errorf("expected: %v, got: %v. Index: %d", expected[i], res[i], i)
		}
	}
}

func TestMatrix_Set(t *testing.T) {
	m := &Matrix{
		rows: 3,
		cols: 3,
		data: []int{1, 2, 3, 4, 5, 6, 7, 8, 9}}
	tData := map[string]struct {
		row, col, value int
		expectedResult  bool
	}{
		"row is less than 0":             {-3, 3, 100, false},
		"row is bigger than matrix.rows": {5, 3, 100, false},
		"col is less than 0":             {2, -3, 100, false},
		"col is bigger than matrix.cols": {2, 5, 100, false},
		"success":                        {2, 0, 100, true},
	}

	for name, tcase := range tData {
		v := tcase
		t.Run(name, func(t *testing.T) {
			result := m.Set(v.row, v.col, v.value)
			if result != v.expectedResult {
				t.Errorf("expected: %v, got: %v", v.expectedResult, result)
			}
		})
	}
}
