# 对象的比较

- C++中是有重载运算符的，然后就可以直接调用sort排序了。

- 而java没有运算符重载，但是也是可以实现对象的比较的。

    两种方式：
        
        1、实现Comparable接口，来对集合中的自定义对象排序
        
        2、实现Comparator接口，来对集合中的自定义对象排序，该方式相对于上一种方式优势在于可以按多种规则排序
        
- 用例（以讨厌的青蛙中 Point类为例）

     Comparable接口：
     
        class Point implements Comparable<Point> {
            private int x;
            private int y;
        
            public int compareTo(Point point) {
                if (this.getX() > point.getX()) {
                    return 1;
                }
        
                if (this.getX() == point.getX() && this.getY() > point.getY()) {
                    return 1;
                }
        
                return 0;
            }
        }
     
   Comparator接口：
     
      class Point implements Comparator<Point> {
        private int x;
        private int y;

        public int compare(Point o1, Point o2) {
           return 0;
        }
      }
        
- 比较

    1、可以看到都是多态接口实现，否则参数是Object类型
    
    2、后者接口更灵活
    

- 总结：C++中的运算符重载可以通过这两个接口实现
