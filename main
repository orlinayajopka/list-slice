package main

import (
	"log"
	"math/rand"
	"sync"
)

var (
	numOfGo    int64 = 50
	numOfNotes int64 = 500
)

type Cache struct {
	mx *sync.Mutex
	ch []int64
}

func writer(ch *Cache, wg *sync.WaitGroup) {
	defer wg.Done()
	for i := int64(0); i < numOfNotes; i++ {
		n := rand.Intn(500000)
		ch.Add(int64(n))
	}
}
func (ch *Cache) Add(num int64) {
	ch.mx.Lock()
	defer ch.mx.Unlock()
	ch.ch = append(ch.ch, int64(num))
}
func main() {
	var wg sync.WaitGroup = sync.WaitGroup{}
	ch := Cache{
		ch: []int64{},
	}
	wg.Add(int(numOfGo))
	for i := int64(0); i < numOfGo; i++ {
		go writer(&ch, &wg)
	}
	wg.Wait()
	log.Println("Должно быть записано", numOfNotes*numOfGo, "значений")
	log.Println("Записано", len(ch.ch), "Значений")
}
