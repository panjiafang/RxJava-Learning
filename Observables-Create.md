## Creating Observables

1. ### Create

   ```java
   public static <T> Observable<T> create(OnSubscribe<T> f) {
       return new Observable<T>(hook.onCreate(f));
   }
   ```

2. ### Defer

   ```java
   public static <T> Observable<T> defer(Func0<Observable<T>> observableFactory) {
       return create(new OnSubscribeDefer<T>(observableFactory));
   }
   ```

3. ### Empty/Error/Never

   ```java
   public static <T> Observable<T> empty() {
       return EmptyObservableHolder.instance();
   }

   public static <T> Observable<T> error(Throwable exception) {
       return create(new OnSubscribeThrow<T>(exception));
   }

   public static <T> Observable<T> never() {
       return NeverObservableHolder.instance();
   }
   ```

4. ### From

   ```java
   public static <T> Observable<T> from(Iterable<? extends T> iterable) {
       return create(new OnSubscribeFromIterable<T>(iterable));
       return create(new OnSubscribeFromArray<T>(array));
   }
   public static <T> Observable<T> from(T[] array) {
       int n = array.length;
       if (n == 0) {
           return empty();
       } else
       if (n == 1) {
           return just(array[0]);
       }
       return create(new OnSubscribeFromArray<T>(array));
   }

   public static <T> Observable<T> from(Future<? extends T> future) {
       return (Observable<T>)create(OnSubscribeToObservableFuture
                                    .toObservableFuture(future));
   }
   public static <T> Observable<T> from(Future<? extends T> future, long timeout, TimeUnit unit) {
       return (Observable<T>)create(OnSubscribeToObservableFuture
                                    .toObservableFuture(future, timeout, unit));
   }
   ```

5. ###Interval

   ```java
   public static Observable<Long> interval(long initialDelay, long period, TimeUnit unit, Scheduler scheduler) {
       return create(new OnSubscribeTimerPeriodically(initialDelay, period, unit, scheduler));
   }
   ```

6. ###Just

   ```java
   public static <T> Observable<T> just(final T value) {
       return ScalarSynchronousObservable.create(value);
   }
   public static <T> Observable<T> just(T t1, T t2) {
       return from((T[])new Object[] { t1, t2 });
   }
   ```

7. ###Range

   ```java
   public static Observable<Integer> range(int start, int count) {
       if (count < 0) {
           throw new IllegalArgumentException("Count can not be negative");
   	}
       if (count == 0) {
           return Observable.empty();
   	}
       if (start > Integer.MAX_VALUE - count + 1) {
           throw new IllegalArgumentException(
               "start + count can not exceed Integer.MAX_VALUE");
   	}
       if(count == 1) {
           return Observable.just(start);
   	}
       return Observable.create(new OnSubscribeRange(start, start + (count - 1)));
   }

   public static Observable<Integer> range(int start, int count, Scheduler scheduler) {
       return range(start, count).subscribeOn(scheduler);
   }
   ```

8. ###Repeat

   ```java
   public final Observable<T> repeat() {
       return OnSubscribeRedo.<T>repeat(this);
   }

   public final Observable<T> repeat(Scheduler scheduler) {
       return OnSubscribeRedo.<T>repeat(this, scheduler);
   }

   public final Observable<T> repeat(final long count) {
       return OnSubscribeRedo.<T>repeat(this, count);
   }

   public final Observable<T> repeat(final long count, Scheduler scheduler) {
       return OnSubscribeRedo.<T>repeat(this, count, scheduler);
   }
   ```

9. ###Timer

   ```java
   public static Observable<Long> timer(long delay, TimeUnit unit) {
       return timer(delay, unit, Schedulers.computation());
   }

   public static Observable<Long> timer(long delay, TimeUnit unit, Scheduler scheduler) {
       return create(new OnSubscribeTimerOnce(delay, unit, scheduler));
   }
   ```

10. ###Start

   `Java中没有实现`


