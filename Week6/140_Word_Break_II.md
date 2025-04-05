# 140. Word Break II

```C++
class Solution {
public:
    bool search(std::string_view s, const vector<string>& words,
                vector<string>& result, string& current) {
        if (s.empty()) {
            result.push_back(current);  // Found a valid sentence
            return true;
        }

        for (const auto& word : words) {
            // If the remaining string starts with a valid word
            if (s.starts_with(word)) {
                size_t curr_len = current.size();

                // Add space if current is not empty
                if (!current.empty()) current += " ";
                current += word;

                // Recursively search for the rest
                search(s.substr(word.size()), words, result, current);

                // Backtrack: undo append
                current.erase(curr_len);
            }
        }

        return false; // Not used, but kept for compatibility
    }

    vector<string> wordBreak(string s, vector<string>& words) {
        vector<string> result;
        string current;
        search(std::string_view(s), words, result, current);
        return result;
    }
};

```