#include <iostream>
#include <iomanip>
#include <map>
#include <limits>
 
template<typename K, typename V>
class interval_map {
    std::map<K,V> m_map;
 
public:
 
    interval_map( V const& val) {
        m_map.insert(m_map.end(),std::make_pair(std::numeric_limits<K>::lowest(),val));
    }
 
    void assign( K const& keyBegin, K const& keyEnd, V const& val ) {
        if (!(keyBegin < keyEnd)) return;
 
        std::pair<K,V> begin;
        std::pair<K,V> end;
        bool beginHas = false;
        bool endHas = false;
        typename std::map<K,V>::iterator itBegin;
        for (itBegin = m_map.begin(); itBegin != m_map.end(); ++itBegin) {
            if (keyBegin < itBegin->first) {
                if (itBegin != m_map.begin()) {
                    beginHas = true;
                    --itBegin;
                    begin = std::make_pair(itBegin->first, itBegin->second);
                }
                break;
            }
        }
        typename std::map<K,V>::iterator itEnd;
        for (itEnd = m_map.begin(); itEnd != m_map.end(); ++itEnd) {
            if (keyEnd < itEnd->first) {
                endHas = true;
                typename std::map<K,V>::iterator extraIt = itEnd;
                --extraIt;
                end = std::make_pair(keyEnd, extraIt->second);
                break;
            }
        }
 
        bool insertMid = true;
        if (beginHas) {
            if (begin.second == val)
                insertMid = false;
        } else {
            if (itBegin != m_map.begin()) {
                typename std::map<K,V>::iterator beforeMid = itBegin;
                --beforeMid;
                if (beforeMid->second == val)
                    insertMid = false;
            }
        }
 
        if (endHas) {
            if ((insertMid && end.second == val) || (!insertMid && end.second == begin.second))
                endHas = false;
        } else {
            typename std::map<K,V>::iterator it;
            for (it = itEnd; it != m_map.end(); ++it) {
                if (insertMid && it->second == val) {
                    endHas = true;
                    break;
                } else if (!insertMid && it->second == begin.second) {
                    itEnd = m_map.erase(itEnd, ++it);
                    break;
                }
            }
        }
        itBegin = m_map.erase(itBegin, itEnd);
        if (beginHas)
            itBegin = m_map.insert(itBegin, begin);
        if (insertMid)
            itBegin = m_map.insert(itBegin, std::make_pair(keyBegin, val));
        if (endHas)
            m_map.insert(itBegin, end);
    }
 
    V const& operator[]( K const& key ) const {
        return ( --m_map.upper_bound(key) )->second;
    }
};
 
// Test function to verify the functionality of the interval_map class
void IntervalMapTest() {
    // Test for empty intervals
    interval_map<int, char> N('A');
    N.assign(3, 3, 'B'); // This interval is empty
 
    // Display the values associated with different keys
    for (int i = 0; i <= 5; ++i) {
        std::cout << i << " -> " << N[i] << std::endl;
    }
 
    // Test for overlapping intervals and removal of redundant entries
    interval_map<int, char> M('A');
    M.assign(1, 5, 'B');
    M.assign(3, 7, 'C');
    M.assign(6, 9, 'D');
 
    // Display the values associated with different keys
    for (int i = 0; i <= 10; ++i) {
        std::cout << i << " -> " << M[i] << std::endl;
    }
 
    // Test for mixing types
    interval_map<double, std::string> O("Default");
    O.assign(1.5, 3.5, "Interval1");
    O.assign(2.0, 4.0, "Interval2");
 
    // Display the values associated with different keys
    for (double i = 1.0; i <= 4.5; i += 0.5) {
        std::cout << i << " -> " << O[i] << std::endl;
    }
 
    // Test for key type limits
    interval_map<unsigned int, char> P('A');
    P.assign(0, 10, 'B');
 
    // Display the values associated with different keys
    for (unsigned int i = 0; i <= 15; ++i) {
        std::cout << i << " -> " << P[i] << std::endl;
    }
}
 
int main()
{
    IntervalMapTest(); // Run the IntervalMap test function
 
    return 0;
}

